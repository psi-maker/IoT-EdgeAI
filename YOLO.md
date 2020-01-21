
YOLOv1 to YOLOv3 structures and evolution  
---
# YOLOV1

原理
输入一张图片，要求输出
1)	其中所包含的对象
2)	以及每个对象的位置（包含该对象的矩形框）。
RCNN系列是两阶段处理模式(先提出候选区，再识别候选区中的对象，还要对候选区进行微调，使之更接近真实的bounding box，最后这个步骤过程就是边框回归); 
Faster RCNN等一些算法采用每个grid中手工设置n个Anchor（先验框，预先设置好位置的bounding box）的设计，每个Anchor有不同的大小和宽高比。YOLO的bounding box看起来很像一个grid中2个Anchor，但它们不是。YOLO并没有预先设置2个bounding box的大小和形状，也没有对每个bounding box分别输出一个对象的预测。它的意思仅仅是对一个对象预测出2个bounding box，选择预测得相对比较准的那个。
这里采用2个bounding box，有点不完全算监督算法，而是像进化算法。如果是监督算法，我们需要事先根据样本就能给出一个正确的bounding box作为回归的目标。但YOLO的2个bounding box事先并不知道会在什么位置，只有经过前向计算，网络会输出2个bounding box，这两个bounding box与样本中对象实际的bounding box计算IOU。这时才能确定，IOU值大的那个bounding box，作为负责预测该对象的bounding box。
训练开始阶段，网络预测的bounding box都是随机的，但总是选择IOU相对好一些的那个，随着训练的进行，每个bounding box会逐渐擅长对某些情况的预测（可能是对象大小、宽高比、不同类型的对象等）。所以，这是一种进化或者非监督学习的思想。
YOLO将Faster RCNN的两个阶段合二为一，使用预测区来代替候选区，将图片划分为 7*7=49 个网格（grid），每个网格允许预测出2个边框（bounding box，包含某个对象的矩形框），总共 49*2=98 个bounding box候选区，它们很粗略的覆盖了图片的整个区域。YOLO的结构非常简单，就是单纯的卷积、池化最后加了两层全连接。和普通的CNN对象分类网络最大的差异是最后输出层用线性函数做激活函数，因为需要预测bounding box的位置（数值型），而不仅仅是对象的概率。YOLO的整个结构就是输入图片经过神经网络的变换得到一个输出的张量。

       
输入样本输入输出的对应关系最后的输出信息，每个网格对应的30维向量

	YOLOv1支持识别20种不同的对象
	每个bounding box需要4个数值来表示其位置，(Center_x,Center_y,width,height)，即(bounding box的中心点的x坐标，y坐标，bounding box的宽度，高度)，2个bounding box共需要8个数值来表示其位置
	bounding box的置信度 = (该bounding box内存在对象的概率 * 该bounding box与该对象实际bounding box的IOU)；意味着它 是否包含对象且位置准确的程度
	IOU是在训练阶段计算出的
	具体就是计算出该Object的bounding box的中心位置，这个中心位置落在哪个grid，该grid对应的输出向量中该对象的类别概率是1（该gird负责预测该对象），所有其它grid对该Object的预测概率设为0（不负责预测该对象）
每个30维向量中只有一组（20个）对象分类的概率，也就只能预测出一个对象。所以输出的 7*7=49个 30维向量，最多表示出49个对象。每个30维向量中有2组bounding box，所以总共是98个候选区。
方法
1)	对于输入图像中的每个对象bouding box, 先找到其中心点和中心点所在的网格，这个网格对应的30维向量中，计算出某一物体的概率是1，则其它对象的概率是0。这就是所谓的"中心点所在的网格对预测该对象负责"。
2)	训练样本的bounding box位置应该填写对象实际的bounding box(根据网络输出的bounding box与对象实际bounding box的IOU来选择)，要在训练过程中动态决定到底填哪一个bounding box
3)	对于2个bounding box的置信度
conf = Pr(obj)* $ IOU ^{truth pred}$
IOU计算方法, IOU大的bounding box其Pr(obj)=1来负责预测某个对象是否存在,就是这个bounding box的conf=$ IOU ^{truth pred}$
 
在训练过程中等网络输出以后，比较两个bounding box与某个物体实际位置的IOU，物体的位置（实际bounding box）放置在IOU比较大的那个bounding box（图中假设是bounding box1），且该bounding box的置信度设为1。
4)	损失函数：损失是网络实际输出值与样本标签值之间的偏差
   
* 网络输出与样本标签的各项内容的误差平方和作为一个样本的整体误差  
* $1_i^obj$是网格i中存在对象;   
* $1_ij^obj$是网格i的第j个bounding box中存在对象;  
* $1_ij^noobj$是网格i的第j个bounding box中不存在对象;  
* 对于公式最后一行’对象分类的误差’， 存在对象的网格才计入误差；  
* 公式第1行和第2行$1_i^obj$，意味着只有"负责"（IOU比较大）预测的那个bounding box的数据才会计入误差  
* 第2行宽度和高度先取了平方根，因为如果直接取差值的话，大的对象对差值的敏感度较低，小的对象对差值的敏感度较高，所以取平方根可以降低这种敏感度的差异，使得较大的对象和较小的对象在尺寸误差上有相似的权重。  
* 乘以$\lambda_coord$ 调节bounding box位置误差的权重（相对分类误差和置信度误差）。YOLO设置 $\lambda_coord$=5，即调高位置误差的权重  
* 第3行是存在对象的bounding box的置信度误差。带有$1_ij^obj$ 意味着只有"负责"（IOU比较大）预测的那个bounding box的置信度才会计入误差  
* 第4行是不存在对象的bounding box的置信度误差。也就是输出尽量低的置信度。如果它不恰当的输出较高的置信度，会与真正"负责"该对象预测的那个bounding box产生混淆。其实就像对象分类一样，正确的对象概率最好是1，所有其它对象的概率最好是0  
* 第4行会乘以$\lambd_noobj$调节不存在对象的bounding box的置信度的权重（相对其它误差）。YOLO设置$\lambd_noobj$=0.5，即调低不存在对象的bounding box的置信度误差的权重  
5)	训练：YOLO的最后一层采用线性激活函数，其它层都是Leaky ReLU。训练中采用了drop out和数据增强（data augmentation）来防止过拟合
6)	Inference: 训练好的YOLO网络，输入一张图片，将输出一个 7*7*30 的张量（tensor）来表示图片中所有网格包含的对象（概率）以及该对象可能的2个位置（bounding box）和可信程度（置信度）。 为了从中提取出最有可能的那些对象和位置，YOLO采用NMS（Non-maximal suppression，非极大值抑制）算法
YOLOV2
 
训练主要包括三个阶段：
* 第一阶段先在ImageNet分类数据集上预训练Darknet-19，此时模型输入为 224*224 ，共训练160个epochs
* 第二阶段将网络的输入调整为 448*448 ，继续在ImageNet数据集上finetune分类模型，训练10个epochs，此时分类模型的top-1准确度为76.5%，而top-5准确度为93.3%  
* 第三个阶段就是修改Darknet-19分类模型为检测模型，移除最后一个卷积层、global avgpooling层以及softmax层，并且新增了三个 3*3*1024卷积层，同时增加了一个passthrough层  
* 最后使用 1*1 卷积层输出预测结果，输出的channels数为：num_anchors*(5+num_classes) ，和训练采用的数据集有关系。由于anchors数为5，对于VOC数据集（20种分类对象）输出的channels数就是125，最终的预测矩阵T的shape为 (batch_size, 13, 13, 125)，可以先将其reshape为 (batch_size, 13, 13, 5, 25) ，其中 T[:, :, :, :, 0:4] 为边界框的位置和大小（t_x,t_y,t_w,t_h），T[:, :, :, :, 4] 为边界框的置信度，而 T[:, :, :, :, 5:] 为类别预测值。
优化
相对v1版本，预测更准确（Better），速度更快（Faster），识别对象更多（Stronger），识别更多对象扩展到能够检测9000种不同对象，称之为YOLO9000。

在神经网络的每一层，在网络（线性变换）输出后和激活函数（非线性变换）之前增加一个批归一化层（BN）。批归一化有助于解决反向传播过程中的梯度消失和梯度爆炸问题，降低对一些超参数（比如学习率、网络参数的大小范围、激活函数的选择）的敏感性，并且每个batch分别进行归一化的时候，起到了一定的正则化效果（YOLO2不再使用dropout），从而能够获得更好的收敛速度和收敛效果。

BN层进行如下变换：  
* 对该批样本的各特征量（对于中间层来说，就是每一个神经元）分别进行归一化处理，分别使每个特征的数据分布变换为均值0，方差1。从而使得每一批训练样本在每一层都有类似的分布。这一变换不需要引入额外的参数。  
* 对上一步的输出再做一次线性变换，假设上一步的输出为Z，则Z1=γZ + β。这里γ、β是可以训练的参数。增加这一变换是因为上一步骤中强制改变了特征数据的分布，可能影响了原有数据的信息表达能力。增加的线性变换使其有机会恢复其原本的信息。  

YOLO2在采用 224*224 图像进行分类模型预训练后，再采用 448*448 的高分辨率样本对分类模型进行微调（10个epoch），使网络特征逐渐适应 448*448 的分辨率。然后再使用 448*448 的检测样本进行训练，缓解了分辨率突然切换造成的影响。
移除了全连接层。另外去掉了一个池化层，使网络卷积层输出具有更高的分辨率。
YOLO1并没有采用先验框，并且每个grid只预测两个bounding box，整个图像98个。YOLO2每个grid采用9个先验框，总共有13*13*9=1521个先验框。 
聚类提取先验框尺度，而非提前手工设定。
YOLO2提出了Darknet-19（有19个卷积层和5个MaxPooling层）网络结构。DarkNet-19比VGG-16小一些，精度不弱于VGG-16，但浮点运算量减少到约1/5，以保证更快的运算速度。

YOLO2引入一种称为passthrough层的方法在特征图中保留一些细节信息。就是在最后一个pooling之前，特征图的大小是26*26*512，将其1拆4，直接传递（passthrough）到pooling后（并且又经过一组卷积）的特征图，两者叠加到一起作为输出的特征图。特征图先用1*1卷积从 26*26*512 降维到 26*26*64，再做1拆4并passthrough。
  
去掉了全连接层，YOLO2可以输入任何尺寸的图像。因为整个网络下采样倍数是32，作者采用了{320,352,...,608}等10种输入图像的尺寸，这些尺寸的输入图像对应输出的特征图宽和高是{10,11,...19}。训练时每10个batch就随机更换一种尺寸，使网络能够适应各种大小的对象检测。
识别分类方法
由于支持更多的分类，并且分类之间有可能是包含关系，对象分类用Logistic取代了softmax（互斥分类）
 
整个WordTree中的对象之间不是互斥的关系，但对于单个节点，属于它的所有子节点之间是互斥关系。比如terrier节点之下的Norfolk terrier、Yorkshire terrier、Bedlington terrier等，各品种的terrier之间是互斥的，所以子节点计算上可以进行softmax操作。
约束预测边框的位置
借鉴并优化了Faster RCNN的先验框方法：System predicts bounding boxes using dimension clusters as anchor boxes . The network predicts 4 coordinates for each bounding box t_x,t_y,t_w,t_h,. If the cell is offset from the top left corner of the image by (cx, cy) and the bounding box prior has width and height pw,ph, then the predictions correspond to:
    

* 虚线是先验框，实线是预测框; 预测边框的蓝色中心点被约束在蓝色背景的网格内。约束边框位置使得模型更容易学习，且预测更为稳定  
*YOLOv2调整了预测公式，将预测边框的中心约束在特定gird网格内  
* $P_r$(obj)*IOU(b,obj)= $\sigma$ ($t_0$)是预测边框的置信度; YOLO1是直接预测置信度的值, YOLOv2对预测参数$t_0$进行$\sigma$变换后作为置信度的值, $\sigma$是sigmoid函数  
* c_x,c_y是当前网格左上角到图像左上角的距离，要先将网格大小归一化，即令一个网格的宽=1，高=1 
* p_w,p_h是先验框的宽和高  
* t_x,t_y,t_w,t_h, t_0是要学习的参数，分别用于预测边框的中心和宽高, 以及置信度  
* b_x,b_y,b_w,b_h是预测边框的中心和宽高  
* 由于$\sigma$函数将t_x,t_y约束在(0,1)范围内，所以根据上面的计算公式，预测边框的蓝色中心点被约束在蓝色背景的网格内。约束边框位置使得模型更容易学习，且预测更为稳定。
误差函数
误差依然包括边框位置误差、置信度误差、对象分类误差
 
* $1_MaxIOU$<Thresh指 预测边框中与真实对象边框IOU最大的那个，其IOU<阈值Thresh，此系数为1，即计入误差，否则为0，不计入误差。YOLO2使用Thresh=0.6  
* $1_t$<12800是前128000次迭代计入误差。注意这里是与先验框的误差，而不是与真实对象边框的误差。是为了在训练早期使模型更快学会先预测先验框的位置  
* $1_k^truth$是该边框负责预测一个真实对象（边框内有对象）。各种$\lambda$是不同类型误差的调节系数  



YOLOV3
https://pjreddie.com/darknet/yolo/
 
* 调整了网络结构；利用多尺度特征进行对象检测    
* 采用了称之为Darknet-53的网络结构（含有53个卷积层），它借鉴了残差网络residual network的做法，在一些层之间设置了快捷链路（shortcut connections）。YOLO2曾采用passthrough结构来检测细粒度特征，在YOLO3更进一步采用了3个不同尺度的特征图来进行对象检测   
* Darknet-53相比于其它网络结构实现了每秒最高的浮点数计算量，说明其网络结构可以更好的利用GPU  
* 残差模块有利于解决深层次网络的梯度问题，每个残差模块由两个卷积层和一个shortcut connections，
1,2,8,8,4代表有几个重复的残差模块，整个v3结构里面，没有池化层和全连接层，网络的下采样是通过设置卷积的stride为2来达到的，每当通过这个卷积层之后图像的尺寸就会减小到一半。而每个卷积层的实现又是包含     卷积+BN+Leaky relu  ,每个残差模块之后又要加上一个zero padding  
 
* 预测对象类别时不使用softmax，改成使用logistic的输出进行预测。这样能够支持多标签对象（比如一个人有Woman 和 Person两个标签）  
* FPN检测方法
* ssd从依据多个feature map来做预测,但是底层的layer并没有选中做object detetion.底层的具有high resolution,但是不具备高级语义high semantic.ssd为了提高速度,在predict的时候不用比较底层的feature map.这一点也导致了它对小目标的检测效果不好.  
* FPN本身并不是object detetcor.它只是一个feature detetor.  
* FPN with RPN，FPN with Fast R-CNN or Faster R-CNN通过FPN,生成了feature map的金字塔(也就是一堆不同尺寸的特征图,都具有高级语义).然后用RPN生成ROI.然后对不同尺寸的目标,选用不同尺寸的特征图去做识别.小目标要用大尺寸的feature map. 大目标用小尺寸的feature map.
 

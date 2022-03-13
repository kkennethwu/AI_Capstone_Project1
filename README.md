# 2022 AI Capstone Project 1 
## Non-Image Dataset
* 這次non-image選擇的是UCI的**Dry-Bean-Dataset**，利用豆子的不同特性來為豆子的種類做分類
* 以下為豆子的7種類別，**['DERMASON', 'SIRA', 'SEKER', 'HOROZ', 'CALI', 'BARBUNYA', 'BOMBAY']**
* data shape = **(13611, 17)**
* 再進行分類時做之前，我將data先切20%當作test_data，剩下的再依K-Fold的K值分成train_data和val_data。
### Analysis
1. Compare the results among **Random Forest** and **KNN**.
    可以發現Random Forest的效果比KNN好很多，而KNN在預測類別**DERMASON**Precision和Recall都非常低，感覺也有可能是test split分得不平均造成這種結果。
![](https://i.imgur.com/Ssa3VET.png =50%x)![](https://i.imgur.com/lCgGp1a.png =50%x)
2. Compare the results with different amount of training data.
    這次選擇的是用Random Forest model，因為我有做K-fold，所以直接更改K值(**K = 2** and **K = 5**)，來做不同大小的training data分析。會發現K=2時，因為所以val_data的部分會比K=5大，所以Validation accuracy比K=5低是合理的。而最後test的部分我認為差accuracy差0.002其實非常小，所以整體比較下來還算合理。
    ![](https://i.imgur.com/ndFdts7.png =50%x)![](https://i.imgur.com/UWnvNBH.png =50%x)
3. Compare the results using different settings and/or hyper-parameters for the same classifier.
    我針對Random Forest的**n_estimator**, **max_depth**, **min_samples_leaf**做改動比較，發現最主要的變因是max_depth，想想也是有道理，因為random forest就是很多顆decision tree，而decison tree的深度也絕對是影響準度的最大原因，當max_depth基本上會越準，但也不能真的太誇張啦有可能會over_fit。而n_esimator越多其實也只是讓預測更平均，min_leaf_samples只要不要改動到太誇張基本上predict結果不會有太大的改變。
    ![](https://i.imgur.com/HJ95OvP.png =50%x)![](https://i.imgur.com/8DzOCN6.png =50%x)
4. (Optional) Compare the results with and without dimensionality reduction (such as PCA) of the features when using high-dimensional data.
    在做PCA比較以前，我偏向認為PCA對這個dataset是沒什麼助益的，因為一班來說需要降維的原因通常是data set維度太高，而samples又不夠多，但Dry Bean Dataset並沒有以上問題，所以感覺做完PCA accuracy反而會降低。而我的實驗結果也剛好符合的我的預測。
    ![](https://i.imgur.com/4S4SfpD.png =50%x)![](https://i.imgur.com/rVEfyW0.png =50%x)
    
### Discussion
1. Based on your experiments, are the results and observed behaviors what you expect?
    基本上從我上面的Analysis，可以發現results和expect滿吻合的，都是滿符合我一值以來學到的AI觀念。
2. Discuss factors that affect the performance, including dataset characteristics.
    就dataset characteristics而言，從model為KNN來觀察的話，可以發現label **SIRA**在precision, recall, confusion matrix裡觀察下來是最容易預測的，影響performance最大的label莫屬**DERMASON**，在precision, recall, confusion matrix裡的表現都不太好，就像上面所提到的，可能是因為在切分dataset時造成樣本不平均，然後model剛好又是KNN，所以performance才影響這麼大，因為KNN也是一個很容易受樣本不平均影響的model，同樣一個label在Random Forest裡就不會有這種狀況。
    除此之外，影響performance的因素還有很多，像是更改hyper parameter也是一種，而analysis也有提到所以就不贅述。
3. Describe experiments that you would do if there were more time available.
    第一個主要是想嘗試，Radom Forest的所有hpyer parameter怎麼調整會得到最好的performace，預期的方法可以用grid serach試試。除此之外，因為上面有提到KNN容易受樣本不平均影響，所以可以多試幾種切分方法，來看怎麼切分可以得到最好的performance。
4. Indicate what you have learned from the experiments and remaining questions.
    因為從前沒有做過這麼大量且繁雜的比較，我認為這個experiments可以很好的幫我們驗證之前學過的理論。


## Image Dataset
* Image Dataset 選擇做的是Kaggle上的**Flowers Recognition**，
* data shape = **(4317, 80, 60, 3)**
* 主要以圖片分類 **['dandelion', 'tulip', 'rose', 'daisy', 'sunflower']** 五種花。
* 我將data先切30%當作test_data，剩下的再依K-Fold的K值分成train_data和val_data。
* 因為其實這個多類別的分類並不容易，再加上本身電腦的限制，我只能夠統一把電腦resize成(64, 64)，所以訓練效果其實並不好，而且網路上很多做法都是用resemble learning和用一些人家寫好的大model(ex. ResNet..)準確率才會高，但因為是比較性質所以我先採用**SVM**和**簡易的CNN**來實作。
1. Compare the results among different classifiers
    以下為**有做HOG的SVM(左)** 和**沒做HOG的簡易CNN(右)** 做比較，可以發現光是這樣CNN效果就已經有超過50%了，所以可能這種multi-class的真的要以SVM來做真的就偏困難。
    ![](https://i.imgur.com/g1b1gzK.png =50%x)![](https://i.imgur.com/kJplc4S.png =50%x)
    以下為上述CNN的架構:
    ![](https://i.imgur.com/cKLi7BC.png) 
2. Compare the results using different settings and/or hyper-parameters for the same classifier.
    以下我以上述的CNN架構為基礎，epoch = 10，改變**, optimizer**做比較。可以得到Adam的準確率又高一些。
    ![](https://i.imgur.com/PGDCv5p.png =50%x)![](https://i.imgur.com/HuyLXHD.png =50%x)

3. Compare the results with different amount of training data.
    以下為**2-Fold(左)** vs **5-Fold(右)**
    ![](https://i.imgur.com/o9oYKFz.png =50%x)![](https://i.imgur.com/ekBqZV1.png =50%x)


### Discussion
1. Based on your experiments, are the results and observed behaviors what you expect?
    我覺得這樣檢測下來，其實圖片的multi-classes還滿困難的，平常要套一些VGG、RESNET等別人寫好的model都很容易，但實際自己操作內部要提高perforamance需要常識非常多東西。
2. Discuss factors that affect the performance, including dataset characteristics.
    從recall, precision來看，我覺得我的CNN model對於**label = 'rose' & 'tulip'** 的預測較不準確
3.  Describe experiments that you would do if there were more time available.
    我想嘗試用別人寫好的model做做看，還有這些model之間的比較。以及試看看所謂的ensembel learning，是否對這個model有幫助。
4. Indicate what you have learned from the experiments and remaining questions.
    這種multi-classes的照片用sklearn的model真的會不准很多(也有可能只是這個dataset的問題)，然後有一個滿大的問題是，有點搞不懂為什麼spec上說的CNN還做cross validation，第一個相對來說滿花時間的，而我認為答案的結果也不會因為cross validation有很大的落差，以CP值來說真的不高。

## Self-made Dataset
* 我選擇的要做的題目是 **Forest Fire Detection**，這個題目的dataset採集容易，而且自己label也很方便，即為 **[fire, nofire]**。
* 這次蒐集的dataset有直接到網路上找forest fire的照片外，也有利用爬蟲的方式採集google上的圖片(這邊是用別人寫好的python package)。除了fire照片外，nofire的照片也很重要，我特別找了像**forest fog、sunset、sunrise、orange** 等與火焰容易混淆的照片，來測試整個model()。
* 而我將整個data切分成，**30% test_data, 剩下的再依K-Fold切分成train_data和validation data**。
* 採用的model分別為**SVM和CNN**。
### Analysis
1. (Optional) Compare the results with and without dimensionality reduction (such as PCA) of the features when using high-dimensional data. **(non-HOG vs HOG)**
    以下分別為，model同樣是SVC，對data不做HOG(左)和做(右)的比較，做過HOG過後Accuracy有明顯提高，因為之前為了讓照片可以成data frame格式，我將照片讀成的array做faltten()，這導致data frame的維度很高，因此相對說sample數又不夠多，所以預測就較會不準確。而且做完HOG後因為維度降低，所以會快很多。
    ![](https://i.imgur.com/sWsc4Cu.png =50%x)![](https://i.imgur.com/8RUoLsa.png =50%x)
而接下來的比較我都會以有做過HOG後比較為主。
2. Compare the results with different amount of training data.
    這次以做過HOG後且model為SVC為基準，變更K-Fold的K值(**K = 2** and **K = 5**)，來做不同大小的training data分析。可以發現兩者在validation的部分差異不大，然後在test的部分，5-Fold優於2-Fold。我認為是因為2-Fold可用的training data較少，所以造成在test時performance比較低。
    ![](https://i.imgur.com/8RUoLsa.png =50%x)![](https://i.imgur.com/dGIcGSZ.png =50%x)
3. Compare the results using different settings and/or hyper-parameters for the same classifier.
    比較的基準依舊為做過HOG後且model為SVC，這次分別更改的部分為**kernel=linear(左) or poly(右)**，而若選擇kernel為poly那就會有**gamma, coef0, degree**需要作調控，所以在這三個參數上我又做了**Grid Search**來尋找最好的結果。可以發現後者的表現比前者好。
    ![](https://i.imgur.com/8RUoLsa.png =50%x)![](https://i.imgur.com/O8iIoFx.png =50%x)
4. Compare the results among different classifiers.
    這裡我拿常做圖像辨識的CNN，和SVM做比較。以下是data尚未做HOG，直接丟進CNN做Validation/ Predict的結果，可以發現，兩者的準確率都比已經做過HOG再用SVM預測來的高。
    ![](https://i.imgur.com/2DxOol9.png =50%x)![](https://i.imgur.com/DFMN9ru.png =50%x)
    以下為把照片丟進CNN的結果：
    ![](https://i.imgur.com/rVyCTHf.png =50%x)![](https://i.imgur.com/T3L4msC.png =50%x)

### Discussion
1. Based on your experiments, are the results and observed behaviors what you expect?
    我原本以為火焰這種東西因為樣子比較隨意不固定，所以做了HOG後也沒有辦法像人臉偵測那樣更好的提升準確率，沒想到完全打臉我的想法。所以再得到這個結果後，我接著把橘子的照片丟進SVC model預測看看，結果不論是否做HOG，預測結果都為nofire，原本以為SVC在沒做HOG的情況會把橘子預測為失火，沒想到我竟然在做HOG前就避免掉這種問題了，看來是我一開始就有把一些很像失火的照片加入train data中達成的效果。
2. Discuss factors that affect the performance, including dataset characteristics.
    我認為主要影響performance的原因是nofire的照片，可以先從confusion matrix看起，將0(沒失火)預測1(失火)，比將1預測為0來的比例高。所以舉例來說像一些橘黃色的東西、像煙霧的東西都很有機會被預測為失火，所以這也才是我一開始要加一堆像失火的照片進train data。
    ![](https://i.imgur.com/ZWodnBK.png)
3. Describe experiments that you would do if there were more time available.
    尋找會讓效果變最好的feature extractor，或是自己寫寫看。
4. Indicate what you have learned from the experiments and remaining questions.
    主要是feature extractor，因為我自己算滿常碰image相關的AI題目，雖然之前上課都有聽過但都沒有實際操作過，結果一試不只效果變好，速度也加快，覺得是個可以研究的方向。所剩的問題應該就是到底要做到怎樣，model才能完全把像失火但不是失火的照片預測正確，這是一個可以探討的問題。
5. 附上利用CNN model和opencv做的application，還滿有趣的XD(原本想丟gif檔過來發現pdf沒辦法支援)
    ![](https://i.imgur.com/8ttIjKQ.png =50%x)![](https://i.imgur.com/vxAAevi.png =50%x)


---
## Code Appendix

https://github.com/KennethWu69/AI_Capstone_Project1
```
Project 1
└───non-image dataset
    └───Dry_Bean_Dataset.xslx
    └───non_image.ipynb
└───image dataset
    └───image_svm.ipynb
    └───image_cnn.ipynb
└───self-made dataset
    └───collect_img.ipynb (爬照片)
    └───simple_images (存取爬下來的照片)
    └───image_dataset
    └───self_made_svm.ipynb
    └───self_made_CNN.ipynb
    
```


    
    
    












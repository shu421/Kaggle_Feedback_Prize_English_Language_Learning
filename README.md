# [Kaggle_Feedback_Prize_English_Language_Learning](https://www.kaggle.com/competitions/feedback-prize-english-language-learning/overview)

- チームメンバーの[wanwan7123](https://www.kaggle.com/tomoyayanagi)、[momijiro](https://www.kaggle.com/momijiro)には大変お世話になりました。

# タスク
- 自然言語処理の回帰問題
- 8-12年生が書いた4000件程度の英語のエッセイに対して、凝集力、構文、語彙、言い回し、文法、定型表現の6項目をそれぞれ1-5点で評価
- 評価指標はMCRMSE
<img width="246" alt="スクリーンショット 2022-11-30 18 29 54" src="https://user-images.githubusercontent.com/71954051/204759011-05b51c0b-fa17-4926-a1b4-e8be91dff994.png">


# 結果
128/2740チーム 銀メダル

# 実験
- 全ては書ききれないので、ベストシングルモデルとアンサンブルを簡単にまとめておきます。
- それぞれの実験についての詳細は[twitter](https://twitter.com/shu421_)のDMで聞いていただければお答えします。

### ベストシングルモデル
- main/exp171.ipynb
  - deberta-v3-large
  - lr: 2e-5
  - fold: 10
  - Loss: SmoothL1Loss
  - max_len: 1024
  - Layerwise Learning Late Decay: 0.95
  - Fast Gradient Method(FGM): 1e-6
  - Mixout: 0.2
  - AttentionPooling
  - reinit last layer
  - cv: 0.4483
  
### アンサンブル
- main/inference_nelder_mead.ipynb
  - 16 models
  - モデル名の隣の数字はnelder-meadで決めた重み
    - deberta-v3-base * 4: 0.13979888, 0.04273566, 0.0373383, 0.03537284
      - max_len, fgm/awpなどを変更
    - deberta-v3-large * 2: 0.19755535, 0.18795201
      - awp/fgm
    - deberta-v3-small: -0.05067628
    - deberta-v2-xlarge: 0.07783308
    - deberta-base: -0.05698146
    - funnel-medium/small: 0.05206032, 0.02667988
    - longformer-base: -0.06565026
    - electra-large: 0.03219258
    - muppet-roberta-large: 0.21354381
    - roberta-base: 0.11499817
    - bart-base: -0.02472768
  - モデル学習時と同じfoldに対して15回nelder-meadを行い、平均値を推論に使用する。
  - cv: 0.44338
  
  



  
    
 

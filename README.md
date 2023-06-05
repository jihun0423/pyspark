# pyspark

이전 프로젝트때 데이터 크기가 큰 데이터셋으로 정제 및 분석을 해본 적이 있었는데, 전처리할 때도 시간이 생각보다 소요됐고, 특히 머신러닝 모델을 통하여 학습할때 시간이 매우 걸려서 다양한 시도를 하지 못했었다.  
이때 이 문제를 해결하고자 방법을 찾던 중, spark를 이용하여 분산처리를 하면 속도가 향상된다는 것을 알게 되었지만, 그때 당시는 학습할 시간이 없어 설치만 해두었었다.  
앞으로 다양한 데이터들을 접하게 될텐데, 그 때를 대비하고자 pyspark를 학습하기로 하였다.


현재 듣고 있는 강의에서는 Databricks를 사용하여 실습을 하였지만, databricks는 무료 버전의 경우 들어갈 때마다 클러스터를 생성하여 실행해 주어야 하기 때문에, 시간이 걸려서 로컬에 pyspark를 설치하여 사용하기로 하였고, 성공적으로 설치하였다.  


* 05.14 : pyspark를 로컬에 설치 후 사용하던 중, sql function을 쓰던 중 오류가 발생하여 원인을 찾아보니 하둡의 경로를 환경변수에 추가하지 않아서 생긴 것이였다. 오류를 해결 한 후 sql function을 사용해 보았는데, 기존에 알고있던 sql과 굉장히 유사해서 처음엔 익숙하게 할 수 있을거라고 생각하였지만, 사용할 수록 약간의 차이점들이 쌓이고 쌓여서 속도가 매우 더뎌졌다. pyspark에서의 문법에 익숙해 지는게 중요할 것 같다. 
* 05.18 : pyspark에서의 문법에 대해서 어느정도 학습은 마쳤지만 여전히 코드를 직접 짤때 사소하게 틀리는 경우가 많다. 특히, pandas dataframe에서는 행 삭제 같은 것이 매우 간편하게 drop을 통해 이루어 졌다면, pyspark dataframe에서는 행 삭제 기능은 일체 없고, 오히려 필터링을 통하여 삭제하고 싶은 행을 걸러내는 느낌으로 사용해야 한다는 것 때문에 평소 하던 습관처럼 코드를 짤 수가 없어 불편했다.
* 05.20 : 이제 pyspark dataframe에서 기초적인 문법은 다 학습하였다. 최근에 plotly, dash나 시계열 데이터를 다루는데 시간을 할애하여 학습 속도가 너무 더뎠던 것에 대해서 반성한다. 
* 05.23 : pyspark에서 sql 문법을 그대로 사용하는 방법이 있다는 걸 알게 된 후 사용해 보았다! 역시 기존 문법을 사용하는게 훨씬 편리했다. pyspark는 pandas와 sql을 섞어 놓은 느낌인데, 섞어놔서 오히려 더 불편하다고 느낀 경우가 많기 때문이다.
* 05.26 : Spark Machine Learning의 특징에 대해서 공부를 하였다. 기존에 익숙했던 sklearn과 매우 유사하지만, 가끔 다른 점이 눈에 띄었다. sklearn과는 달리, 설명변수들을 하나의 vector로 묶는 vectorization을 해 주어야 하는 것 같다. 또 특이했던 점은, 보통 전처리 도중 라벨 인코딩이나 표준화 할때 사용하던 fit, transform을 Spark ML에서는 모델에서도 사용한다는 것이였다. fit을 통해 학습한 모델을 불러와 주고, 이 모델을 통해 테스트 데이터의 데이터 프레임을 transform을 통해 변환을 해주는 형식이다.
* 06.01 : SparkML을 실제로 사용해보니, 오히려 sklearn보다 더 편한 부분도 보이는 느낌이다. sklearn에서 머신러닝 모델을 학습시키기 전에, 항상 사람 이름이나 나라 이름 같은 범주형 변수들을 제거 해 줘야하고, 학습을 한 뒤에도 테스트 할 때 나중에 concat을 통해 합하여야 확인을 할 수 있다는게 불편했는데, SparkML에서는 애초에 처음부터 모델에 사용할 변수들을 하나의 Vector로 변환해서 그 Vector만 사용하기 때문에, 다른 필요없지만 보고 싶은 변수들을 제거하지 않아도 된다는 점이 좋았다.
* 06.04 : 학습하면 할 수록 sklearn과 다르게 불편한 점들이 매우 많다. 문자형에서 바로 one-hot 인코딩이 안되기 때문에 먼저 라벨 인코딩을 해 준 뒤에야 가능하다는 점, 표준화나 정규화를 할 때 반드시 데이터의 형식이 vector 이여야 한다는 점 (vectorization을 해주어야 한다. sklearn처럼 그냥 컬럼을 지목해서 하려고 하면 타입 오류가 뜬다.) 등 pandas에 익숙한 나에게는 굉장히 번거롭고 짜증나는 과정들이다. 그나마 장점이라면 Pipeline을 작성하면 매우 깔끔하게 실행 할 수 있다는 점이지만... 여전히 번거로운건 사실이다. 이렇게 되면 기존의 데이터에 추가로 표준화된 컬럼들까지 추가되는 형식인 것이라서 용량도 늘어나지 않을까 하는 우려도 있다. (물론 기존 컬럼들을 제거하는 과정을 추가한다면 문제는 없겠지만 이것도 직접 추가해야 한다..)

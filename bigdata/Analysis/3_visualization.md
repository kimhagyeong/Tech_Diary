## matplotlib
- 시각화를 위한 파이썬 라이브러리.
- pandas의 데이터프레임, 시리즈 자료구조와 함께 사용 가능
- 따라서 데이터 처리와 동시에 시각화도 함께 진행할 수 있음
- 아나콘다(anaconda)를 설치했다면 별도의 설치과정이 필요 없음

<pre><code>
import matplotlib.pyplot as plt
import pandas as pd
from pandas import DataFrame
from pandas import Series

# matplotlib 한글 폰트 출력코드
# 출처 : 데이터공방( https://kiddwannabe.blog.me)

import matplotlib
from matplotlib import font_manager, rc
import platform

try : 
    if platform.system() == 'Windows':
    # 윈도우인 경우
        font_name = font_manager.FontProperties(fname="c:/Windows/Fonts/malgun.ttf").get_name()
        rc('font', family=font_name)
    else:    
    # Mac 인 경우
        rc('font', family='AppleGothic')
except : 
    pass
matplotlib.rcParams['axes.unicode_minus'] = False   
</code></pre>
- matplotlib documentation : https://matplotlib.org/stable/tutorials/index.html#introductory


## Seaborn
- matplotlib을 기본으로 다양한 시각화 기법을 제공하는 라이브러리.
- 엄청나게 화려한 시각화 기법들을 제공하며, 기본적으로 이쁩니다.  
histplot, kdeplot, jointplot, Facetgrid, ...
- pandas DataFrame과 매우 호환이 잘 됩니다.  
e.g. sns.xxxplot(data=df) <--- 기본세팅
<pre><code>
# 라이브러리와 데이터를 불러오고, 시각화를 위한 세팅을 합니다.
import seaborn as sns
sns.set_theme(style='whitegrid')
penguins = sns.load_dataset("penguins")
</code></pre>
- seaborn documentation : https://seaborn.pydata.org/introduction.html

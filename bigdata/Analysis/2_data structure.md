## Numpy

- numpy는 "numerical python"의 약자입니다.  
Numerical Computing : 컴퓨터가 실수값을 효과적으로 계산할 수 있도록 하는 연구 분야. Vector Arithmetic : 벡터 연산 --> 데이터가 벡터로 표현되기 때문이다.  
  
- numpy는 다양한 머신러닝 라이브러리들에 의존성을 가지고 있고, 일반 파이썬 리스트에 비해 강력한 성능을 자랑합니다.  
성능 : numpy array >> python list(or tuple). 
  
- python list와 비슷한 개념을 numpy에서는 numpy array라고 부른다.  
파이썬 리스트처럼 여러 데이터를 한번에 다룰 수 있으나, 모든 데이터가 동일한 data type을 가져야합니다.  
  
- C언어와 JAVA에서 사용하는 array와 비슷한 개념이며, 동적 할당(dynamic type binding)을 지원하는 파이썬의 리스트와 구조가 다릅니다.  
  
- Numpy의 특징  
1) numpy array는 모든 원소의 자료형이 동일해야 한다.
2) numpy array는 선언할 때 크기를 지정한 뒤, 변경할 수 없다.  
list.append(), pop()을 통해 자유롭게 원소 변경 및 크기 변경이 가능하지만, numpy array는 만들어지고 나면 원소의 update는 가능하지만, array의 크기를 변경할 수는 없다.
3) 사실 numpy array는 C, C++로 구현이 되어 있다. 이는 high performance를 내기 위해서이며, python이 Numerical computing에 취약하다는 단점을 보완한다.
4) numpy array가 python list보다 빠른 이유 중에 하나는 원소의 type checking을 할 필요가 없기 때문이다.
5) numpy array는 universal function(through broadcast)를 제공하기 때문에 같은 연산 반복에 대해 훨씬 빠르다. 데이터의 크기가 클수록 차이가 더 크다.

- Numpy basics
https://doorbw.tistory.com/171

-  Array Operation (like vector) --> Universal Function. 
numpy array를 쓰는 가장 큰 이유는 vector처럼 사용할 수 있기 때문입니다.  
그렇기 때문에 scipy, matplotlib, scikit-learn, pandas, tensorflow, pytorch 등 대부분의 데이터분석 라이브러리들이 numpy array를 사용합니다.  
대부분의 데이터 분석 라이브러리들은 벡터를 사용하는데, 그 벡터가 바로 numpy array로 표현되기 때문입니다.  
데이터 분석은 99.9% 데이터를 벡터로 표현하여 분석하기 때문에, 이 특징은 굉장히 중요합니다.  
벡터 == numpy array. 
  
  
  
## Pandas
> #### 간단히 말하면  
> - 데이터 분석 및 처리를 위한 파이썬 라이브러리  
> - 데이터분석에 최적화된 자료구조와 함수가 내장  
> - 데이터사이언티스트들이 현업에서 주로 사용하는 라이브러리  
  
  
- pandas는 "python data analysis"의 약자입니다.  
pandas는 정형 데이터 처리에 특화되어 있다.  

- pandas 역시 다양한 머신러닝 라이브러리들에 의존성을 가지고 있습니다.  
scikit-learn, scipy, statsmodel, tensorflow, pytorch, ...
  
- 간단하게 생각하면, python에서 excel의 기능을 사용할 수 있게 됩니다.  
pandas = python + excel // pandas & excel // pandas VS MS Excel
  
- 하지만, pandas는 numpy array를 베이스로 지원하며 파이썬과 함께 강력한 시너지를 내기 때문에, 엑셀 그 이상의 퍼포먼스를 냅니다.  
pandas가 Excel에 비해 고성능 데이터처리에 적합하다.
- Pandas 라이브러리에서 기본적으로 데이터를 다루는 단위는 DataFrame입니다. 흔히 알고있는 spreadsheet와 같은 개념입니다.  
- 이러한 형태의 데이터는 Structured Data 또는 Panel Data 또는 Tabular Data라고 부릅니다.
- pandas를 공부한다는 것은 결국 dataframe의 사용법을 익히고 활용하는 방법을 배운다는 것과 같습니다.
- pandas를 잘 활용하면 대부분의 structured data를 자유자재로 다룰 수 있게 됩니다.  
- Pandas의 기본 자료구조(Series, DataFrame). 
DataFrame은 2차원 테이블이고, 테이블의 한 줄(행/열)을 Series라고 합니다.  
Series의 모임이 곧, DataFrame이 됩니다.  
- pandas documents : https://pandas.pydata.org/docs/user_guide/dsintro.html


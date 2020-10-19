### 텍스트 전처리

- 문장에서 괄호를 제외한 부분만 추출하기
- 추출한 문장은 new 컬럼으로 저장
- 새로운 데이터프레임은 csv 파일로 저장 

### Text Pre-processing

- Getting the sentence without bracket
- The sentence after pre-processing will be saved in column ```new``` 
- The new dataframe will be saved as CSV file


```python
#reading excel
import pandas as pd
```


```python
#전체 데이터 확인
dialect=pd.read_excel('./대본종합_200929.xlsx')
dialect.tail(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>화일번호 :</th>
      <th>대본종합_200929</th>
      <th>Unnamed: 2</th>
      <th>Unnamed: 3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>28593</th>
      <td>28593</td>
      <td>일본에게 뽄때(본때)를 보여주자.</td>
      <td>일상대화</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28594</th>
      <td>28594</td>
      <td>매정하게 배신한 남자에게 뽄때(본때)를 보여주자.</td>
      <td>연애/결혼</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### 정규표현식 설명
```
\( : 괄호로 시작
[^)] :닫는 괄호를 제외한 모든 글자들 
* : 여러개의 글자가능
\) : 괄호로 끝남
```


```python
# new 컬럼 : 괄호를 제외한 새로운 문장 
dialect['new']=dialect['대본종합_200929'].replace(r'\([^)]*\)','',regex=True)
```


```python
dialect.tail(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>화일번호 :</th>
      <th>대본종합_200929</th>
      <th>Unnamed: 2</th>
      <th>Unnamed: 3</th>
      <th>new</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>28593</th>
      <td>28593</td>
      <td>일본에게 뽄때(본때)를 보여주자.</td>
      <td>일상대화</td>
      <td>NaN</td>
      <td>일본에게 뽄때를 보여주자.</td>
    </tr>
    <tr>
      <th>28594</th>
      <td>28594</td>
      <td>매정하게 배신한 남자에게 뽄때(본때)를 보여주자.</td>
      <td>연애/결혼</td>
      <td>NaN</td>
      <td>매정하게 배신한 남자에게 뽄때를 보여주자.</td>
    </tr>
  </tbody>
</table>
</div>




```python
dialect.loc[5]
```




    화일번호 :                                         005
    대본종합_200929    그래도 그런 놈은 여븐뎅이루(옆으로) 써도 매련(형편) 없는데.
    Unnamed: 2                                     NaN
    Unnamed: 3                                     NaN
    new                     그래도 그런 놈은 여븐뎅이루 써도 매련 없는데.
    Name: 5, dtype: object



### 적용 & csv 생성


```python
dialect.to_csv('preprocessed_dialect.csv')
```


```python
new_dialect=pd.read_csv('./preprocessed_dialect.csv')
new_dialect.tail(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>화일번호 :</th>
      <th>대본종합_200929</th>
      <th>Unnamed: 2</th>
      <th>Unnamed: 3</th>
      <th>new</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>28590</th>
      <td>28590</td>
      <td>28590</td>
      <td>회사갈때 정장을 입어야 뽄때(본때)가 나지.</td>
      <td>회사/학교</td>
      <td>NaN</td>
      <td>회사갈때 정장을 입어야 뽄때가 나지.</td>
    </tr>
    <tr>
      <th>28591</th>
      <td>28591</td>
      <td>28591</td>
      <td>학교에서는 공부잘하는기 뽄때(본때) 있는기다.</td>
      <td>회사/학교</td>
      <td>NaN</td>
      <td>학교에서는 공부잘하는기 뽄때 있는기다.</td>
    </tr>
    <tr>
      <th>28592</th>
      <td>28592</td>
      <td>28592</td>
      <td>뽄때(본때)를 보여주는건 좋지만 거짓말 하면 욕합니다.</td>
      <td>일상대화</td>
      <td>NaN</td>
      <td>뽄때를 보여주는건 좋지만 거짓말 하면 욕합니다.</td>
    </tr>
    <tr>
      <th>28593</th>
      <td>28593</td>
      <td>28593</td>
      <td>일본에게 뽄때(본때)를 보여주자.</td>
      <td>일상대화</td>
      <td>NaN</td>
      <td>일본에게 뽄때를 보여주자.</td>
    </tr>
    <tr>
      <th>28594</th>
      <td>28594</td>
      <td>28594</td>
      <td>매정하게 배신한 남자에게 뽄때(본때)를 보여주자.</td>
      <td>연애/결혼</td>
      <td>NaN</td>
      <td>매정하게 배신한 남자에게 뽄때를 보여주자.</td>
    </tr>
  </tbody>
</table>
</div>



# STRING, DATE Problem

[링크](https://programmers.co.kr/learn/courses/30/parts/17045)

> 출제 빈도 - 낮음

> 평균 점수 - 보통

<br>

# Table Description

`ANIMAL_INS` 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. <br>
`ANIMAL_INS` 테이블 구조는 다음과 같으며, <br>
`ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE` <br>
는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

| NAME | TYPE | NULLABLE |
| :-: | :-: | :-: |
| ANIMAL_ID | VARCHAR(N) | FALSE |
| ANIMAL_TYPE |	VARCHAR(N)	| FALSE |
| DATETIME	| DATETIME | FALSE |
| INTAKE_CONDITION |	VARCHAR(N)	| FALSE |
| NAME |	VARCHAR(N) |	TRUE |
| SEX_UPON_INTAKE |	VARCHAR(N)	| FALSE |

<br>
<br>
<br>
<br>
<br>

## 1. 이름이 없는 동물의 아이디

[링크](https://programmers.co.kr/learn/courses/30/lessons/59039)

<br>

동물 보호소에 들어온 동물 중, 이름이 없는 채로 들어온 동물의 ID를 조회하는 SQL 문을 작성해주세요. 단, ID는 오름차순 정렬되어야 합니다.

예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면

| ANIMAL_ID	| ANIMAL_TYPE |	DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A368930 | Dog | 2014-06-08 13:20:00 | Normal | NULL | Spayed Female |
| A524634 | Dog | 2015-01-02 18:54:00 | Normal | *Belle | Intact Female |
| A465637 | Dog | 2017-06-04 08:17:00 | Injured | *Commander | Neutered Male |

<br>

이름이 없는 채로 들어온 동물의 ID는 A368930입니다. 따라서 SQL을 실행하면 다음과 같이 출력되어야 합니다.

| ANIMAL_ID |
| :-: |
| A368930 |

<br>
<br>

> ## Solution

```SQL
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NULL;
```

<br>
<br>
<br>
<br>
<br>

## 2. 이름이 있는 동물의 아이디

[링크](https://programmers.co.kr/learn/courses/30/lessons/59407)

<br>

동물 보호소에 들어온 동물 중, 이름이 있는 동물의 ID를 조회하는 SQL 문을 작성해주세요. 단, ID는 오름차순 정렬되어야 합니다.

예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면

<br>

| ANIMAL_ID	| ANIMAL_TYPE |	DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A434523 | Cat | 2015-11-20 14:18:00 | Normal | NULL | Spayed Female |
| A562649 | Dog | 2014-03-20 18:06:00 | Sick | NULL | Spayed Female |
| A524634 | Dog | 2015-01-02 18:54:00 | Normal | *Belle | Intact Female |
| A465637 | Dog | 2017-06-04 08:17:00 | Injured | *Commander | Neutered Male |

<br>

이름이 있는 동물의 ID는 A524634와 A465637입니다. 따라서 SQL을 실행하면 다음과 같이 출력되어야 합니다.

| ANIMAL_ID |
| :-: |
| A465637 | 
| A524634 |

<br> 
<br> 

> ### Solution
```SQL
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NOT NULL;
```
	
<br>
<br>
<br>
<br>
<br>

## 3. NULL 처리하기

[링크](https://programmers.co.kr/learn/courses/30/lessons/59410)

<br>

입양 게시판에 동물 정보를 게시하려 합니다. 동물의 생물 종, 이름, 성별 및 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요. 이때 프로그래밍을 모르는 사람들은 NULL이라는 기호를 모르기 때문에, 이름이 없는 동물의 이름은 "No name"으로 표시해 주세요.

예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A350276 | Cat | 2017-08-13 13:50:00 | Normal | Jewel | Spayed Female |
| A350375 | Cat | 2017-03-06 15:01:00 | Normal | Meo | Neutered Male |
| A368930 | Dog | 2014-06-08 13:20:00 | Normal | NULL | Spayed Female |

<br>

마지막 줄의 개는 이름이 없기 때문에, 이 개의 이름은 "No name"으로 표시합니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

| ANIMAL_TYPE | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: |
| Cat | Jewel | Spayed Female |
| Cat | Meo | Neutered Male |
| Dog | No name | Spayed Female |

<br>

※ 컬럼 이름은 일치하지 않아도 됩니다.

<br>
<br>

> ### Solution

```sql
SELECT ANIMAL_TYPE, NVL(NAME, 'No name'), SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

```SQL
SELECT 
    ANIMAL_TYPE, 
    CASE WHEN NAME IS NULL THEN 'No name' 
        ELSE NAME END AS NAME,
    SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

<br>
<br>
<br>
<br>
<br>

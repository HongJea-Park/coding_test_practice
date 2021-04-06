# STRING, DATE Problem

[링크](https://programmers.co.kr/learn/courses/30/parts/17046)

> 출제 빈도 - 낮음

> 평균 점수 - 낮음

<br>

# Table Description(1~4)

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

## 1. 루시와 엘라 찾기

[링크](https://programmers.co.kr/learn/courses/30/lessons/59046)

<br>
동물 보호소에 들어온 모든 동물의 정보를 `ANIMAL_ID`순으로 조회하는 SQL문을 작성해주세요.

이때 결과는 아이디 순으로 조회해주세요. 예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면

| ANIMAL_ID	| ANIMAL_TYPE |	DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A373219 | Cat | 2014-07-29 11:43:00 | Normal | Ella | Spayed Female |
| A377750 | Dog | 2017-10-25 17:17:00 | Normal | Lucy | Spayed Female |
| A353259 | Dog | 2016-05-08 12:57:00 |	Injured | Bj | Neutered Male |
| A354540 | Cat | 2014-12-11 11:48:00 | Normal | Tux | Neutered Male |
| A354597 | Cat | 2014-05-02 12:16:00 | Normal | Ariel | Spayed Female |

<br>

SQL문을 실행하면 다음과 같이 나와야 합니다.

| ANIMAL_ID | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: |
| A373219 | Ella | Spayed Female |
| A377750 | Lucy | Spayed Female |

<br>
<br>

> ## Solution

```SQL
SELECT I.ANIMAL_ID, I.NAME, I.SEX_UPON_INTAKE
FROM ANIMAL_INS I
WHERE I.NAME IN ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty')
ORDER BY I.ANIMAL_ID;
```

<br>
<br>
<br>
<br>
<br>

## 2. 이름에 el이 들어가는 동물 찾기

[링크](https://programmers.co.kr/learn/courses/30/lessons/59047)

<br>

보호소에 돌아가신 할머니가 기르던 개를 찾는 사람이 찾아왔습니다. 이 사람이 말하길 할머니가 기르던 개는 이름에 'el'이 들어간다고 합니다. 동물 보호소에 들어온 동물 이름 중, 이름에 "EL"이 들어가는 개의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 이름 순으로 조회해주세요. 단, 이름의 대소문자는 구분하지 않습니다.

예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면

<br>

| ANIMAL_ID	| ANIMAL_TYPE |	DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A355753 | Dog | 2015-09-10 13:14:00 | Normal | Elijah | Neutered Male |
| A352872 | Dog | 2015-07-09 17:51:00 | Aged | Peanutbutter | Neutered Male |
| A353259 | Dog | 2016-05-08 12:57:00 | Injured | Bj | Neutered Male |
| A373219 | Cat | 2014-07-29 11:43:00 | Normal | Ella | Spayed Female |
| A382192 | Dog | 2015-03-13 13:14:00 | Normal | Maxwell 2 | Intact Male |

<br>

- 이름에 'el'이 들어가는 동물은 Elijah, Ella, Maxwell 2입니다.
- 이 중, 개는 Elijah, Maxwell 2입니다.

따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

| ANIMAL_ID | NAME |
| :-: | :-: |
| A355753 | Elijah |
| A382192 | Maxwell 2 |

<br> 
<br> 

> ### Solution
```SQL
SELECT I.ANIMAL_ID, I.NAME
FROM ANIMAL_INS I
WHERE I.NAME LIKE '%el%' AND I.ANIMAL_TYPE = 'Dog'
ORDER BY I.NAME;
```
	
<br>
<br>
<br>
<br>
<br>

## 3. 중성화 여부 파악하기

[링크](https://programmers.co.kr/learn/courses/30/lessons/59409)

<br>

보호소의 동물이 중성화되었는지 아닌지 파악하려 합니다. 중성화된 동물은 `SEX_UPON_INTAKE` 컬럼에 'Neutered' 또는 'Spayed'라는 단어가 들어있습니다. 동물의 아이디와 이름, 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요. 이때 중성화가 되어있다면 'O', 아니라면 'X'라고 표시해주세요.

예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A355753 | Dog | 2015-09-10 13:14:00 | Normal | Elijah | Neutered Male |
| A373219 | Cat | 2014-07-29 11:43:00 | Normal | Ella | Spayed Female |
| A382192 | Dog | 2015-03-13 13:14:00 | Normal | Maxwell 2 | Intact Male |

<br>

- 중성화한 동물: Elijah, Ella
- 중성화하지 않은 동물: Maxwell 2

따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

| ANIMAL_ID | NAME | 중성화 |
| :-: | :-: | :-: |
| A355753 | Elijah | O |
| A373219 | Ella | O |
| A382192 | Maxwell 2 | X |

<br>

※ 컬럼 이름은 일치하지 않아도 됩니다.

<br>
<br>

> ### Solution

```sql
SELECT I.ANIMAL_ID, I.NAME, 
    (CASE 
        WHEN I.SEX_UPON_INTAKE LIKE '%Neutered%' THEN 'O'
        WHEN I.SEX_UPON_INTAKE LIKE '%Spayed%' THEN 'O'
        ELSE 'X' 
        END) AS '중성화'
FROM ANIMAL_INS I
ORDER BY I.ANIMAL_ID;
```

<br>
<br>
<br>
<br>
<br>

## 4. DATETIME에서 DATE로 형 변환

[링크](https://programmers.co.kr/learn/courses/30/lessons/59414)

<br>

`ANIMAL_INS` 테이블에 등록된 모든 레코드에 대해, 각 동물의 아이디와 이름, 들어온 날짜1를 조회하는 SQL문을 작성해주세요. 이때 결과는 아이디 순으로 조회해야 합니다.

예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A349996 | Cat | 2018-01-22 14:32:00 | Normal | Sugar | Neutered Male |
| A350276 | Cat | 2017-08-13 13:50:00 | Normal | Jewel | Spayed Female |
| A350375 | Cat | 2017-03-06 15:01:00 | Normal | Meo | Neutered Male |
| A352555 | Dog | 2014-08-08 04:20:00 | Normal | Harley | Spayed Female |
| A352713 | Cat | 2017-04-13 16:29:00 | Normal | Gia | Spayed Female |

<br>

SQL문을 실행하면 다음과 같이 나와야 합니다.

| ANIMAL_ID | NAME | 날짜 |
| :-: | :-: | :-: |
| A349996 | Sugar | 2018-01-22 |
| A350276 | Jewel | 2017-08-13 |
| A350375 | Meo | 2017-03-06 |
| A352555 | Harley | 2014-08-08 |
| A352713 | Gia | 2017-04-13 |

<br>
<br>

> ### Solution

```sql
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%M-%D') AS '날짜'
FROM ANIMAL_INS
ORDER BY '날짜';
```

<br>
<br>
<br>
<br>
<br>

# Table Description(5)

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

`ANIMAL_OUTS` 테이블은 동물 보호소에서 입양 보낸 동물의 정보를 담은 테이블입니다. <br>
`ANIMAL_OUTS` 테이블 구조는 다음과 같으며, <br>
`ANIMAL_ID, ANIMAL_TYPE, DATETIME, NAME, SEX_UPON_OUTCOME`는 각각 동물의 아이디, 생물 종, 입양일, 이름, 성별 및 중성화 여부를 나타냅니다. <br>
`ANIMAL_OUTS` 테이블의 `ANIMAL_ID`는 `ANIMAL_INS`의 ``ANIMAL_ID`의 외래 키입니다. <br>

| NAME | TYPE | NULLABLE |
| :-: | :-: | :-: |
| ANIMAL_ID | VARCHAR(N) | FALSE |
| ANIMAL_TYPE | VARCHAR(N) | FALSE |
| DATETIME | DATETIME | FALSE |
| NAME | VARCHAR(N) | TRUE |
| SEX_UPON_OUTCOME | VARCHAR(N) | FALSE |

<br>
<br>
<br>
<br>
<br>

## 5. 오랜 기간 보호한 동물(2)

[링크](https://programmers.co.kr/learn/courses/30/lessons/59411)

<br>

입양을 간 동물 중, 보호 기간이 가장 길었던 동물 두 마리의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 기간이 긴 순으로 조회해야 합니다.

예를 들어, `ANIMAL_INS` 테이블과 `ANIMAL_OUTS` 테이블이 다음과 같다면

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A354597 | Cat | 2014-05-02 12:16:00 | Normal | Ariel | Spayed Female |
| A362707 | Dog | 2016-01-27 12:27:00 | Sick | Girly Girl | Spayed Female |
| A370507 | Cat | 2014-10-27 14:43:00 | Normal | Emily | Spayed Female |
| A414513 | Dog | 2016-06-07 09:17:00 | Normal | Rocky | Neutered Male |

<br>

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | NAME | SEX_UPON_OUTCOME |
| :-: | :-: | :-: | :-: | :-: |
| A354597 | Cat | 2014-06-03 12:30:00 | Ariel | Spayed Female |
| A362707 |	Dog | 2017-01-10 10:44:00 | Girly Girl | Spayed Female |
| A370507 | Cat | 2015-08-15 09:24:00 | Emily | Spayed Female |

<br>

SQL문을 실행하면 다음과 같이 나와야 합니다.

| ANIMAL_ID | NAME |
| :-: | :-: |
| A362707 | Girly Girl |
| A370507 | Emily |

<br>
<br>

> ## Solution

```SQL
SELECT O.ANIMAL_ID, O.NAME
FROM ANIMAL_OUTS O
    JOIN ANIMAL_INS I ON I.ANIMAL_ID = O.ANIMAL_ID
ORDER BY O.DATETIME - I.DATETIME DESC LIMIT 2;
```

<br>
<br>
<br>
<br>
<br>

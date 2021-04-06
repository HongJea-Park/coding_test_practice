# JOIN Problem

[링크](https://programmers.co.kr/learn/courses/30/parts/17046)

> 출제 빈도 - 높음

> 평균 점수 - 낮음

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

## 1. 없어진 기록 찾기

[링크](https://programmers.co.kr/learn/courses/30/lessons/59042)

<br>
천재지변으로 인해 일부 데이터가 유실되었습니다. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.

예를 들어, `ANIMAL_INS` 테이블과 `ANIMAL_OUTS` 테이블이 다음과 같다면

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A352713 | Cat | 2017-04-13 16:29:00 | Normal | Gia | Spayed Female |
| A350375 | Cat | 2017-03-06 15:01:00 | Normal | Meo | Neutered Male |

<br>

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | NAME | SEX_UPON_OUTCOME |
| :-: | :-: | :-: | :-: | :-: |
| A349733 | Dog | 2017-09-27 19:09:00 | Allie | Spayed Female |
| A352713 | Cat | 2017-04-25 12:25:00 | Gia | Spayed Female |
| A349990 | Cat | 2018-02-02 14:18:00 | Spice | Spayed Female |

<br>

`ANIMAL_OUTS` 테이블에서

- Allie의 ID는 `ANIMAL_INS`에 없으므로, Allie의 데이터는 유실되었습니다.
- Gia의 ID는 `ANIMAL_INS`에 있으므로, Gia의 데이터는 유실되지 않았습니다.
- Spice의 ID는 `ANIMAL_INS`에 없으므로, Spice의 데이터는 유실되었습니다.

따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

| ANIMAL_ID | NAME |
| :-: | :-: |
| A349733 | Allie |
| A349990 | Spice |

<br>
<br>

> ## Solution

```SQL
SELECT O.ANIMAL_ID, O.NAME
FROM ANIMAL_OUTS O
    LEFT JOIN ANIMAL_INS I ON O.ANIMAL_ID = I.ANIMAL_ID
WHERE I.ANIMAL_ID IS NULL
```

<br>
<br>
<br>
<br>
<br>

## 2. 있었는데요 없었습니다

[링크](https://programmers.co.kr/learn/courses/30/lessons/59043)

<br>

관리자의 실수로 일부 동물의 입양일이 잘못 입력되었습니다. 보호 시작일보다 입양일이 더 빠른 동물의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일이 빠른 순으로 조회해야합니다.

예를 들어, `ANIMAL_INS` 테이블과 `ANIMAL_OUTS` 테이블이 다음과 같다면

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A350276 | Cat | 2017-08-13 13:50:00 | Normal | Jewel | Spayed Female |
| A381217 | Dog | 2017-07-08 09:41:00 | Sick | Cherokee | Neutered Male |

<br>

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | NAME | SEX_UPON_OUTCOME |
| :-: | :-: | :-: | :-: | :-: |
| A350276 | Cat | 2018-01-28 17:51:00 | Jewel | Spayed Female |
| A381217 | Dog | 2017-06-09 18:51:00 | Cherokee | Neutered Male |

<br>

SQL문을 실행하면 다음과 같이 나와야 합니다.

| ANIMAL_ID | NAME |
| :-: | :-: |
| A381217 | Cherokee |

<br> 
<br> 

> ### Solution
```SQL
SELECT I.ANIMAL_ID, I.NAME
FROM ANIMAL_INS I
    JOIN ANIMAL_OUTS O ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE I.DATETIME > O.DATETIME
ORDER BY I.DATETIME;
```
	
<br>
<br>
<br>
<br>
<br>

## 3. 오랜 기간 보호한 동물(1)

[링크](https://programmers.co.kr/learn/courses/30/lessons/59044)

<br>

아직 입양을 못 간 동물 중, 가장 오래 보호소에 있었던 동물 3마리의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일 순으로 조회해야 합니다.

예를 들어, `ANIMAL_INS` 테이블과 `ANIMAL_OUTS` 테이블이 다음과 같다면

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A354597 | Cat | 2014-05-02 12:16:00 | Normal | Ariel | Spayed Female |
| A373687 | Dog | 2014-03-20 12:31:00 | Normal | Rosie | Spayed Female |
| A412697 | Dog | 2016-01-03 16:25:00 | Normal | Jackie | Neutered Male |
| A413789 | Dog | 2016-04-19 13:28:00 | Normal | Benji | Spayed Female |
| A414198 | Dog | 2015-01-29 15:01:00 | Normal | Shelly | Spayed Female |
| A368930 | Dog | 2014-06-08 13:20:00 | Normal | | Spayed Female |

<br>

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | NAME | SEX_UPON_OUTCOME |
| :-: | :-: | :-: | :-: | :-: |
| A354597 | Cat | 2014-05-02 12:16:00 | Ariel | Spayed Female |
| A373687 | Dog | 2014-03-20 12:31:00 | Rosie | Spayed Female |
| A368930 | Dog | 2014-06-13 15:52:00 | | Spayed Female |

<br>

SQL문을 실행하면 다음과 같이 나와야 합니다.

| NAME | DATETIME |
| :-: | :-: |
| Shelly | 2015-01-29 15:01:00 |
| Jackie | 2016-01-03 16:25:00 |
| Benji | 2016-04-19 13:28:00 |

※ 입양을 가지 못한 동물이 3마리 이상인 경우만 입력으로 주어집니다.

<br>
<br>

> ### Solution

```sql
SELECT I.NAME, I.DATETIME
FROM ANIMAL_INS I
    LEFT JOIN ANIMAL_OUTS O ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE O.DATETIME IS NULL
ORDER BY I.DATETIME LIMIT 3;
```

<br>
<br>
<br>
<br>
<br>

## 4. 중복 제거하기

[링크](https://programmers.co.kr/learn/courses/30/lessons/59408)

<br>

보호소에서 중성화 수술을 거친 동물 정보를 알아보려 합니다. 보호소에 들어올 당시에는 중성화1되지 않았지만, 보호소를 나갈 당시에는 중성화된 동물의 아이디와 생물 종, 이름을 조회하는 아이디 순으로 조회하는 SQL 문을 작성해주세요.

예를 들어, `ANIMAL_INS` 테이블과 `ANIMAL_OUTS` 테이블이 다음과 같다면

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | INTAKE_CONDITION | NAME | SEX_UPON_INTAKE |
| :-: | :-: | :-: | :-: | :-: | :-: |
| A367438 | Dog | 2015-09-10 16:01:00 | Normal | Cookie | Spayed Female |
| A382192 | Dog | 2015-03-13 13:14:00 | Normal | Maxwell 2 | Intact Male |
| A405494 | Dog | 2014-05-16 14:17:00 | Normal | Kaila | Spayed Female |
| A410330 | Dog | 2016-09-11 14:09:00 | Sick | Chewy | Intact Female |

<br>

| ANIMAL_ID | ANIMAL_TYPE | DATETIME | NAME | SEX_UPON_OUTCOME |
| :-: | :-: | :-: | :-: | :-: |
| A367438 | Dog | 2015-09-12 13:30:00 | Cookie | Spayed Female |
| A382192 | Dog | 2015-03-16 13:46:00 | Maxwell 2 | Neutered Male |
| A405494 | Dog | 2014-05-20 11:44:00 | Kaila | Spayed Female |
| A410330 | Dog | 2016-09-13 13:46:00 | Chewy | Spayed Female |

<br>

- Cookie는 보호소에 들어올 당시에 이미 중성화되어있었습니다.
- Maxwell 2는 보호소에 들어온 후 중성화되었습니다.
- Kaila는 보호소에 들어올 당시에 이미 중성화되어있었습니다.
- Chewy는 보호소에 들어온 후 중성화되었습니다.

따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.

| ANIMAL_ID | ANIMAL_TYPE | NAME |
| :-: | :-: | :-: |
| A382192 | Dog | Maxwell 2 |
| A410330 | Dog | Chewy |

<br>
<br>

> ### Solution

```sql
SELECT I.ANIMAL_ID, I.ANIMAL_TYPE, I.NAME
FROM ANIMAL_INS I
    JOIN ANIMAL_OUTS O ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE I.SEX_UPON_INTAKE != O.SEX_UPON_OUTCOME;
```

<br>
<br>
<br>
<br>
<br>

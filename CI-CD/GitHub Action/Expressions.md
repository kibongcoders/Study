# Expressions

Expressions을 사용하여 워크플로우 파일에서 환경 변수를 프로그래밍 방식으로 설정하고 컨텍스트에 엑세스 가능합니다.
컨텍스트에 대한 참조 또는 함수의 조합으로 사용 가능하고 연산자를 사용하여 리터럴, 컨텍스트 참조, 함수를 결합할 수 있습니다.

```yaml
env:
  MY_ENV_VAR: ${{ <expression> }}
```

Expressions 사용은 기본적으로 ${{expression}}으로 사용 가능합니다.

## 리터럴

식의 일부로 boolean, null, number, string 데이터 형식을 사용가능합니다.

```yaml
env:
  myNull: ${{ null }}
  myBoolean: ${{ false }}
  myIntegerNumber: ${{ 711 }}
  myFloatNumber: ${{ -9.2 }}
  myHexNumber: ${{ 0xff }}
  myExponentialNumber: ${{ -2.99e-2 }}
  myString: Mona the Octocat
  myStringInBraces: ${{ 'It''s open source!' }}
```


## 연산자

|연산자|설명|
|---|---|
|`( )`|논리적 그룹화|
|`[ ]`|색인|
|`.`|속성 참조 해제|
|`!`|아니다|
|`<`|보다 작음|
|`<=`|보다 작거나 같음|
|`>`|보다 큼|
|`>=`|보다 크거나 같음|
|\==|같음|
|`!=`|같지 않음|
|`&&`|그리고|
|`\|`|또는|

## 주의점

GitHub는 문자열을 비교할 때 대/소문자를 무시합니다.
`steps.<step_id>.outputs.<output_name>`은 문자열로 계산됩니다.
숫자 비교의 경우 `fromJSON()` 함수를 사용하여 문자열을 숫자로 변환할 수 있습니다.

GitHub은 느슨한 동등 비교를 수행합니다.

- 형식이 일치하지 않으면 GitHub는 형식을 숫자로 강제 변환합니다.

|형식|결과|
|---|---|
|없는|`0`|
|부울|`true``1`를 반환합니다  <br>`false``0`를 반환합니다|
|문자열|모든 유효한 JSON 숫자 형식에서 구문 분석됩니다. 그렇지 않으면 `NaN`입니다.  <br>참고: 빈 문자열이 `0`을 반환합니다.|
|배열|`NaN`|
|물체|`NaN`|

- 하나의 `NaN`을 다른 `NaN`와 비교해도 `true`가 나오지 않습니다.
- 개체와 배열은 동일한 인스턴스일 때만 동일하게 간주됩니다.

## 함수

GitHub는 식에서 사용할 수 있는 기본 제공 함수 집합을 제공합니다.
일부 함수는 값을 문자열로 캐스팅하여 비교를 수행합니다.
GitHub는 이러한 변환을 사용하여 데이터 형식을 문자열로 캐스팅합니다.

### contains()

contains(search, item) search가 item을 포함하는 경우 true 반환
search가 배열인 경우 item이 배열의 요소인 경우 true를 반환합니다.

### startsWith(searchString, searchValue)

searchString이 searchValue로 시작하면 true를 반환합니다. 
값을 문자열로 비교해서 비교합니다.
GitHub 대소문자를 구분하지 않는다는 점 기억해야합니다.

### endsWith(searchString, searchValue)

searchString이 searchValue로 끝나면 true를 반환합니다.

### format( string, replaceValue0, replaceValue1, ..., replaceValueN)

`string`의 값을 `replaceValueN` 변수로 바꿉니다.
`replaceValue` 및 `string`을 하나 이상 지정해야 합니다.

```javascript
format('Hello {0} {1} {2}', 'Mona', 'the', 'Octocat')
```

의 경우 ‘Hello Mona the Octocat’을 반환합니다.

### join(array, optionalSeparator)

?? 공부가 필요함

### toJSON(value)

value의 자동 서식 지정을 JSON 표현으로 변환합니다.
`toJSON(job)`은 `{ "status": "success" }`를 반환할 수 있습니다.

```yaml
name: build
on: push
jobs:
  job1:
    runs-on: ubuntu-latest
    outputs:
      matrix: ${{ steps.set-matrix.outputs.matrix }}
    steps:
      - id: set-matrix
        run: echo "matrix={\"include\":[{\"project\":\"foo\",\"config\":\"Debug\"},{\"project\":\"bar\",\"config\":\"Release\"}]}" >> $GITHUB_OUTPUT
  job2:
    needs: job1
    runs-on: ubuntu-latest
    strategy:
      matrix: ${{ fromJSON(needs.job1.outputs.matrix) }}
    steps:
      - run: build
```

해당 .yaml 파일의 경우 matrix json 파일을  fromJSON을 사용하여 다음 작업을 전달합니다.

```yaml
name: print
on: push
env:
  continue: true
  time: 3
jobs:
  job1:
    runs-on: ubuntu-latest
    steps:
      - continue-on-error: ${{ fromJSON(env.continue) }}
        timeout-minutes: ${{ fromJSON(env.time) }}
        run: echo ...
```

해당 파일의 경우 fromJSON 사용하여 JSON 데이터의 값을 가져올 수 있습니다.

### hashFiles(path)

path 패턴과 일치하는 파일 세트에 대한 단일 해시를 반환합니다.
path 패턴 또는 쉼표로 여러 path 패턴을 제공 가능합니다.

path는 GITHUB_WORKSPACE 디렉터리에 상대적입니다.

단일의 경우
`hashFiles('**/package-lock.json')`

여러 패턴의 경우
`hashFiles('**/package-lock.json', '**/Gemfile.lock')`

## 상태 검사 함수

상태검사 함수를 if 조건의 식으로 사용할 수 있습니다.
```yaml
steps:
  ...
  - name: The job has succeeded
    if: ${{ success() }}
```

### success()
이전 step이 성공한 경우 true를 반환합니다.

### always()

취소된 경우에도 단계가 항상 실행되고 true가 반환되게 됩니다.
하지만 심각한 오류가 발생 될 수 있는 작업에는 사용하지 않는것이 좋습니다.
이런 경우 사건이초과될 때 까지 워크플로우가 중단 될 수 있습니다.

### cancelled()

워크플로우가 취소 된 경우 true를 반환합니다.

### failure()

```yaml
steps:
  ...
  - name: The job has failed
    if: ${{ failure() }}
```


## 개체 필터

`*` 을 사용하여 필터를 적용하고 컬렉션에서 일치하는 항목을 선택할 수 있습니다.

```json
[
  { "name": "apple", "quantity": 1 },
  { "name": "orange", "quantity": 2 },
  { "name": "pear", "quantity": 1 }
]
```

해당 배열에서 `fruits.*.name` 필터는 `[ "apple", "orange", "pear" ]` 배열을 반환합니다.

또한 개체의 구문에서도 가능한데
```json
{
  "scallions":
  {
    "colors": ["green", "white", "red"],
    "ediblePortions": ["roots", "stalks"],
  },
  "beets":
  {
    "colors": ["purple", "red", "gold", "white", "pink"],
    "ediblePortions": ["roots", "stems", "leaves"],
  },
  "artichokes":
  {
    "colors": ["green", "purple", "red", "black"],
    "ediblePortions": ["hearts", "stems", "leaves"],
  },
}
```

에 경우 `vegetables.*.ediblePortions` 의 경우 

```json
[
  ["roots", "stalks"],
  ["hearts", "stems", "leaves"],
  ["roots", "stems", "leaves"],
]
```

값을 반환하게 됩니다.
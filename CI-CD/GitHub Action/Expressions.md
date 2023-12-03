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


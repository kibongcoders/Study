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

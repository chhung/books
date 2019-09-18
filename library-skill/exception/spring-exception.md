---
description: 使用spring framework時做error handler
---

# Spring exception

## Scenario

我們寫了一個RESTful API，再回傳給使用者前可以有一些錯誤檢查，且如果有問題則回傳錯誤訊息。\(四則運算\)

{% code-tabs %}
{% code-tabs-item title="request form:" %}
```bash
{"function":"add", "number1":3, "number2":5}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="response form:" %}
```bash
{"isSuccess":true, "result":8}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="response form:" %}
```bash
{"isSuccess":false, "errorCode":520, "errorMessage":"It can't be operation with number."}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## @ExceptionHandler

![&#x7E7C;&#x627F;RuntimeException&#x985E;&#x5225;](../../.gitbook/assets/2019-09-18_152529.jpg)

@ExceptionHandler所宣告處理的方法，只限定在該類別當中，意思是，若其他類別也丟出定義的exception class，那麼將不會被處理；因為找不到，例如，ArithmeticController.java裡面就拋出MathError class之後，會去找要攔截此例外的方法是否在ArithmeticController.java裡面，如果有就按方法的流程處理，如果沒有就回應http status 500。

最上面的那張圖指的是，我們會透過自訂的錯誤類別，再去繼承RuntimeException類別，這樣在程式中拋出例外才不會繼續往下執行；因此，自訂的錯誤類別有主要兩個功用，第一個是在抛出例外前把該錯誤碼及錯誤訊息寫入到錯誤類別；第二個是錯誤分級，可以讓@ExceptionHandler攔截不同的錯誤類別。

{% code-tabs %}
{% code-tabs-item title="ArithmeticController.java" %}
```java
@RestController
@RequestMapping("/operation/v1/")
public class ArithmeticController {
	
	@PostMapping(path = "/math", 
			produces = MediaType.APPLICATION_JSON_UTF8_VALUE, 
			consumes = MediaType.APPLICATION_JSON_UTF8_VALUE)
	public RespMath click(@RequestBody ReqMath body) {
		RespMath response = new RespMath();
		
		if (!body.getFunction().equalsIgnoreCase("add")) {
			MathError ret = new MathError();
			ret.setIsSuccess(false);
			ret.setErrorCode(520);
			ret.setErrorMessage("It can't be operation with number.");
			throw ret;
		}
		
		response.setIsSuccess(true);
		response.setResult(body.getNumber1() + body.getNumber2());
		
		return response;
	}
	
	@ExceptionHandler(MathError.class)
	private ResponseEntity<String> errorHandle(MathError ex) {
		MultiValueMap<String, String> headers = new LinkedMultiValueMap<String, String>();
		headers.add("Content-Type", "application/json;charset=UTF-8");
		
		ResponseEntity<String> response = 
				new ResponseEntity<String>(ex.toString(), headers, HttpStatus.BAD_REQUEST);
		
		return response;
	}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="ReqMath.java" %}
```java
public class ReqMath {
	private String function;
	private Integer number1;
	private Integer number2;
	
	public String getFunction() {
		return function;
	}
	public void setFunction(String function) {
		this.function = function;
	}
	public Integer getNumber1() {
		return number1;
	}
	public void setNumber1(Integer number1) {
		this.number1 = number1;
	}
	public Integer getNumber2() {
		return number2;
	}
	public void setNumber2(Integer number2) {
		this.number2 = number2;
	}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="RespMath.java" %}
```java
public class RespMath {
	private Boolean isSuccess;
	private Integer result;
	
	public Boolean getIsSuccess() {
		return isSuccess;
	}
	public void setIsSuccess(Boolean isSuccess) {
		this.isSuccess = isSuccess;
	}
	public Integer getResult() {
		return result;
	}
	public void setResult(Integer result) {
		this.result = result;
	}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="MathError.java" %}
```java
public class MathError extends RuntimeException {
	private static final long serialVersionUID = 3769051713058814440L;

	private Boolean isSuccess;
	private Integer errorCode;
	private String errorMessage;
	
	@Override
	public String toString() {
		StringBuilder str = new StringBuilder();
		str.append("{")
			.append("\"isSuccess\":").append(isSuccess).append(",")
			.append("\"errorCode\":").append(errorCode).append(",")
			.append("\"errorMessage\":").append("\"").append(errorMessage).append("\"")
			.append("}");
		
		return str.toString();
	}
	public Boolean getIsSuccess() {
		return isSuccess;
	}
	public void setIsSuccess(Boolean isSuccess) {
		this.isSuccess = isSuccess;
	}
	public Integer getErrorCode() {
		return errorCode;
	}
	public void setErrorCode(Integer errorCode) {
		this.errorCode = errorCode;
	}
	public String getErrorMessage() {
		return errorMessage;
	}
	public void setErrorMessage(String errorMessage) {
		this.errorMessage = errorMessage;
	}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## HandlerExceptionResolver







## @ControllerAdvice


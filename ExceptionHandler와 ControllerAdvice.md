## @RestControllerAdvice와 @ExceptionHandler를 통한 예외처리

- 스프링의 모든 @RestController에서 발생하는 예외를 전역적으로 처리
- 특정 컨트롤러가 아닌 전체 컨트롤러
- 예외를 하나의 클래스에서 처리함으로써 코드를 일관성있게 관리할 수 있다.

```java
@RestControllerAdvice
@Slf4j
public class GlobalExceptionHandler {

    @ExceptionHandler(CustomException.class)
    public ResponseEntity<ErrorResponse> handleCustomException(CustomException e) {
        log.error("CustomException : {}, {}", e.getHttpStatus(), e.getMessage());
        ErrorResponse errorResponse = new ErrorResponse(e.getHttpStatus(), e.getMessage());

        return ResponseEntity.status(e.getHttpStatus()).body(errorResponse);
    }

    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<ErrorResponse> handleUnexpectedException(RuntimeException e) {
        log.error("UnexpectedException : {}", e.getMessage());
        ErrorResponse errorResponse = new ErrorResponse(HttpStatus.INTERNAL_SERVER_ERROR,
                e.getMessage());

        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(errorResponse);
    }
```

```java
@Getter
public class ErrorResponse {

    private final int status;
    private final String message;

    public ErrorResponse(HttpStatus status, String message) {
        this.status = status.value();
        this.message = message;
    }

}
```

```java
@Getter
public class CustomException extends RuntimeException {

    private final HttpStatus httpStatus;

    private final String message;

    public CustomException(ErrorInformation errorInformation) {
        this.httpStatus = errorInformation.getHttpStatus();
        this.message = errorInformation.getMessage();
    }
}
```

```java
@Getter
public enum ErrorInformation {

    // 400
    REQUEST_VALIDATION_FAIL(HttpStatus.BAD_REQUEST, "유효한 요청 값이 아닙니다."),

    //403
    ACCESS_DENIED(HttpStatus.FORBIDDEN, "접근 권한 오류입니다."),

    // 404
    ORDER_NOT_FOUND(HttpStatus.NOT_FOUND, "존재하지 않는 주문입니다."),
    USER_NOT_FOUND(HttpStatus.NOT_FOUND, "존재하지 않는 사용자입니다."),

    DELIVERY_NOT_FOUND(HttpStatus.NOT_FOUND, "존재하지 않는 배달 정보입니다."),

    // 409
    DUPLICATE_MEMBER(HttpStatus.CONFLICT, "이미 사용 중인 계정입니다."),

    // 500
    INTERNAL_SERVER_ERROR(HttpStatus.INTERNAL_SERVER_ERROR, "서버 내부 오류입니다. 관리자에게 문의 주세요.");

    private final HttpStatus httpStatus;
    private final String message;

    ErrorInformation(HttpStatus httpStatus, String message) {
        this.httpStatus = httpStatus;
        this.message = message;
    }
}
```

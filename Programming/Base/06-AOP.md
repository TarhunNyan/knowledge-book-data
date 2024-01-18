# AOP

AOP - аспектно ориентированное программирование

# Concept

Общая логика проста:

-   [JoinPoint - места куда можем воткнуться](#aop---joinpoint)
-   [Pointcut - места которые выбрали чтобы воткнуться](#aop---pointcut)
-   [Advice - что втыкаем в желаемое место](#aop---advice)

Обобщение:

-   [Aspect - место в котором объеденины Pointcut и Advice](#aop---aspect)

## AOP - JoinPoint

JoinPoint - точка наблюдения, в которых планируется введение дополнительной функциональности:

-   вызываеющий код - откуда-то вызываем метод
-   целевой код - метод который вызваем

```bash
// было
вызываеющий код -> JoinPoint -> целевой код
```

## AOP - Pointcut

Pointcut - это что-то вроде запроса, который отбирает JoinPoint, куда будет добавленнна новая функциональность:

-   @annotation(AspectAnnotation) - будет вызывать someMethod, для всех методов помеченных аннотацией AspectAnnotation
-   execution(\* com.example.demoAspects.check()) - будет вызывать someMethod2, при вызове com.example.demoAspects.check()

```java
@Pointcut("@annotation(AspectAnnotation)")
void someMethod() { ... }

@Pointcut("execution(* com.example.demoAspects.check())")
void someMethod2() { ... }
```

## AOP - Advice

Advice - набор инструкций выполняемых в Pointcut:

-   Before — перед вызовом метода
-   After — после вызова метода
-   After returning — после возврата значения из функции
-   After throwing — в случае exception
-   After finally — в случае выполнения блока finally
-   Around — предобработка/постобработка/обработку перед вызовом метода/обойти вызов метода

## AOP - Aspect

Aspect - место в котором объеденины Pointcut и Advice:

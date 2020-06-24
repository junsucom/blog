---
layout: post
title: reified in Kotlin
author: junsu
date: 2020-04-02 10:51:00
---
# reified in Kotlin

JAVA 때 Gson 으로 object 를 변환 해주는 function 개발시 항상 Generic(<T>) 선언한다음 param 으로 clazz : Class 를 전달해 줘야 했다.

Kotlin에서도 그렇게 사용 하고 있었는데.. reified 라는 키워드를 알고서는 이것을 해결 하기 위해서 사용 하는것이 구나 라는걸 알게 되었다.

[kotlin 문서(https://kotlinlang.org/docs/reference/inline-functions.html#reified-type-parameters)](https://kotlinlang.org/docs/reference/inline-functions.html#reified-type-parameters)

기존에 사용 하고 있던 retrofit 에서 HttpException 의 errorBody 를 변환 해주던 함수

~~~kotlin
fun <T> HttpException.asErrorBody(clazz: Class<T>): T? {
    return this.response()?.let {
        Gson().fromJson(
            it.errorBody()?.charStream(), clazz
        )
    }
}

//사용시
val errorBody = e.asErrorBody(BaseResponse::class.java)
~~~

변신

~~~kotlin
inline fun <reified T> HttpException.asErrorBody(): T? {
    return this.response()?.let {
        Gson().fromJson(
            it.errorBody()?.charStream(), T::class.java
        )
    }
}

//사용시
val errorBody = e.asErrorBody<BaseResponse>()
~~~

크게 차이는 없지만.. 뭔가 Kotlin 스러워 졌다는 느낌..
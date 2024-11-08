# 고급 의존성

## 매개변수화된 의존성

지금까지 본 모든 의존성은 고정된 함수 또는 클래스입니다.

하지만 여러 개의 함수나 클래스를 선언하지 않고도 의존성에 매개변수를 설정해야 하는 경우가 있을 수 있습니다.

예를 들어, `q` 쿼리 매개변수가 특정 고정된 내용을 포함하고 있는지 확인하는 의존성을 원한다고 가정해 봅시다.

이때 해당 고정된 내용을 매개변수화할 수 있길 바랍니다.

## "호출 가능한" 인스턴스

Python에는 클래스의 인스턴스를 "호출 가능"하게 만드는 방법이 있습니다.

클래스 자체(이미 호출 가능함)가 아니라 해당 클래스의 인스턴스에 대해 호출 가능하게 하는 것입니다.

이를 위해 `__call__` 메서드를 선언합니다:

//// tab | Python 3.9+

```Python hl_lines="12"
{!> ../../docs_src/dependencies/tutorial011_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="11"
{!> ../../docs_src/dependencies/tutorial011_an.py!}
```

////

//// tab | Python 3.8+ non-Annotated

/// tip | 참고

가능하다면 `Annotated` 버전을 사용하는 것이 좋습니다.

///

```Python hl_lines="10"
{!> ../../docs_src/dependencies/tutorial011.py!}
```

////

이 경우, **FastAPI**는 추가 매개변수와 하위 의존성을 확인하기 위해 `__call__`을 사용하게 되며,
나중에 *경로 연산 함수*에서 매개변수에 값을 전달할 때 이를 호출하게 됩니다.

## 인스턴스 매개변수화하기

이제 `__init__`을 사용하여 의존성을 "매개변수화"할 수 있는 인스턴스의 매개변수를 선언할 수 있습니다:

//// tab | Python 3.9+

```Python hl_lines="9"
{!> ../../docs_src/dependencies/tutorial011_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="8"
{!> ../../docs_src/dependencies/tutorial011_an.py!}
```

////

//// tab | Python 3.8+ non-Annotated

/// tip | 참고

가능하다면 `Annotated` 버전을 사용하는 것이 좋습니다.

///

```Python hl_lines="7"
{!> ../../docs_src/dependencies/tutorial011.py!}
```

////

이 경우, **FastAPI**는 `__init__`에 전혀 관여하지 않으며, 우리는 이 메서드를 코드에서 직접 사용하게 됩니다.

## 인스턴스 생성하기

다음과 같이 이 클래스의 인스턴스를 생성할 수 있습니다:

//// tab | Python 3.9+

```Python hl_lines="18"
{!> ../../docs_src/dependencies/tutorial011_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="17"
{!> ../../docs_src/dependencies/tutorial011_an.py!}
```

////

//// tab | Python 3.8+ non-Annotated

/// tip | 참고

가능하다면 `Annotated` 버전을 사용하는 것이 좋습니다.

///

```Python hl_lines="16"
{!> ../../docs_src/dependencies/tutorial011.py!}
```

////

이렇게 하면 `checker.fixed_content` 속성에 `"bar"`라는 값을 담아 의존성을 "매개변수화"할 수 있습니다.

## 인스턴스를 의존성으로 사용하기

그런 다음, `Depends(FixedContentQueryChecker)` 대신 `Depends(checker)`에서 이 `checker` 인스턴스를 사용할 수 있으며,
클래스 자체가 아닌 인스턴스 `checker`가 의존성이 됩니다.

의존성을 해결할 때 **FastAPI**는 이 `checker`를 다음과 같이 호출합니다:

```Python
checker(q="somequery")
```

...그리고 이때 반환되는 값을 *경로 연산 함수*의 `fixed_content_included` 매개변수로 전달합니다:

//// tab | Python 3.9+

```Python hl_lines="22"
{!> ../../docs_src/dependencies/tutorial011_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="21"
{!> ../../docs_src/dependencies/tutorial011_an.py!}
```

////

//// tab | Python 3.8+ non-Annotated

/// tip | 참고

가능하다면 `Annotated` 버전을 사용하는 것이 좋습니다.

///

```Python hl_lines="20"
{!> ../../docs_src/dependencies/tutorial011.py!}
```

////

/// tip | 참고

이 모든 과정이 복잡하게 느껴질 수 있습니다. 그리고 지금은 이 방법이 얼마나 유용한지 명확하지 않을 수도 있습니다.

이 예시는 의도적으로 간단하게 만들었지만, 전체 구조가 어떻게 작동하는지 보여줍니다.

보안 관련 장에서는 이와 같은 방식으로 구현된 편의 함수들이 있습니다.

이 모든 과정을 이해했다면, 이러한 보안 도구들이 내부적으로 어떻게 작동하는지 이미 파악한 것입니다.

///
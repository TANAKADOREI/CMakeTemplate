# CMakeTemplate

모든 언어는 번역되어 작성되었습니다.

すべての言語は翻訳されて作成されました。

All languages have been translated and written

## JP

* "[SOLUTION_NAME]"のように"[]"を含めた名前はユーザーが括弧を含めて消して名前を付けてください。

## EN

* For names that include “[]”, such as “[SOLUTION_NAME]”, the user can name them by erasing parentheses.

## KR

* "[SOLUTION_NAME]"처럼 "[]"를 포함한 이름은 사용자가 괄호를 포함해 지워 이름을 지어주시면 됩니다

# Reference description

## internal project

Reference A to B

|     (A) / (B)      | Native Project | Common Project |
| :----------------: | :------------: | :------------: |
| **Native Project** |       X        |       X        |
| **Common Project** |       O        |       O        |



# Frequently used path

Directory containing native declaration mapping headers.

```
${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__INCLUDE__
```

Library directory location to be used in the selected native environment

```
${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__NATIVE__/__${MB_CURRENT_ENVIRONMENT}__/__LIBS__
```

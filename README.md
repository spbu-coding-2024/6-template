# Задача 6

## Описание

Разработать консольную утилиту **calculator**, которая позволяет вычислять арифметические выражения для целых чисел произвольной длины.

### Операции

Необходимо поддержать следующие операции:

- Сложение
- Вычитание
- Умножение
- _Опционально_: целочисленное деление

### Нотации

Выражение может быть представлено в обратной польской нотации (поддержка обязательна) и инфиксной нотации (поддержка опциональна).

Утилита должна обрабатывать следующие аргументы командной строки для выбора нотации:

- `--revpol` — выражение вводится в обратной польской нотации
  - В этом случае предполагается, что числа с числами, числа с операторами, операторы с числами, разделены пробелами, выражение вводится без скобок.
- _Опционально (инфиксная запись)_: `--infix` — выражение вводится в инфиксной форме
  - В этом случае не предполагается обязательное разделение пробелами. Также в записи выражения могут участвовать скобки.

## Требования к реализации

- Выражение, которое необходимо вычислить, подаётся на стандартный поток ввода
- Хранение чисел должно быть реализовано на **связных списках** (реализованных самостоятельно)
  - В узлах списка нужно хранить разряды числа (основание системы счисления может быть хоть 2, хоть 10, хоть 16, хоть 2^64)
- Должны быть реализованы отрицательные числа
  - способ на усмотрение автора
  - отрицательные числа начинаются с минуса после которого сразу следуют цифры
  - таким образом, вычитание возможно двумя способами:
    - `10 4 -`
    - `10 -4 +`
  - _Опционально (инфиксная запись)_: в инфиксной нотации отрицательное число находится в скобках
    - `10 + (-4)`
    - `(-4) * 5`
    - `10 + ((-4) * 5)`
- _Опционально (деление)_: при выполнении целочисленного деления ответ должен совпадать с результатом, полученным следующими вычислениями:
  1. Необходимо разделить `p` на `q`
  2. При выполнении обычного деления будет получена дробь `p/q`
  3. Результатом будет округление получившейся дроби вниз до ближайшего целого числа, иными словами `floor((double)p / q)`
  </br>
  Примеры:

  - `-1 2 /` </br>
    Ответ: `-1`
  - `2 -3 /` </br>
    Ответ: `-1`
  - `2 3 /` </br>
    Ответ: `0`

## Сценарии и коды возврата

При различных сценариях программа должна завершаться с разными кодами возврата, а также информировать пользователя о некорректности ввода, в том числе в случае отсутствия одного из указанных аргументов или присутствия «мусора».

- При корректном входе приложение должно выводить результат вычисления выражения в стандартный поток вывода и завершаться с кодом возврата **0**;

- При некорректной передаче аргументов командной строки приложение должно вывести соответствующее сообщение об ошибке в стандартный поток ошибок и завершиться с кодом **1**;

- В случае если приложение не поддерживает инфиксную нотацию, при аргументе командной строки `--infix`, приложение должно вывести соответствующее сообщение в стандартный поток ошибок и завершиться с кодом **2**;

- В случае если приложение не поддерживает целочисленное деление, а в выражении присутствует такая операция, приложение должно вывести соответствующее сообщение в стандартный поток ошибок и завершиться с кодом **3**;

- Если при разборе структуры выражения или его вычисления возникли ошибки, необходимо вывести понятное сообщение о сути проблемы в стандартный поток ошибок и завершить программу с кодом возврата **4**;

- Во всех остальных случаях программа должна завершиться с кодом **5**.

## Примеры:

- Запуск: `calculator --revpol`<br/>
  stdin: `6 4 - 7 *`<br/>
  stdout: `14`<br/>
  Код возврата: `0`
- Запуск: `calculator --revpol`<br/>
  stdin: `10 -4 + 7 -`<br/>
  stdout: `-1`<br/>
  Код возврата: `0`
- Запуск: `calculator --revpoch`<br/>
  stderr: `Invalid command line arguments`<br/>
  Код возврата: `1`
- Запуск: `calculator --infix`<br/>
  stderr: `Unsupported notation`<br/>
  Код возврата: `2`
- Запуск: `calculator --revpol`<br/>
  stdin: `3 a + 7 *`<br/>
  stderr: `Invalid character at position 3`<br/>
  Код возврата: `4`
- Запуск: `calculator --revpol`<br/>
  stdin: `3 (-4) + 7 *`<br/>
  stderr: `Invalid character at position 3`<br/>
  Код возврата: `4`
- Запуск: `calculator --revpol`<br/>
  stdin: `3 4 + 7 /`<br/>
  stderr: `Unsupported operation`<br/>
  Код возврата: `3`
- Запуск: `calculator --revpol`<br/>
  stdin: `10 -4 + 7 8`<br/>
  stderr: `Operation symbol is missed`<br/>
  Код возврата: `4`

### Опциональные примеры

- Запуск: `calculator --revpol`<br/>
  stdin: `10 -4 + 7 /`<br/>
  stdout: `0`<br/>
  Код возврата: `0`
- Запуск: `calculator --infix`<br/>
  stdin: `(3 + 4) * 7`<br/>
  stdout: `49`<br/>
  Код возврата: `0`
- Запуск: `calculator --infix`<br/>
  stdin: `(10 + (-4)) / 7`<br/>
  stdout: `0`<br/>
  Код возврата: `0`
- Запуск: `calculator`<br/>
  stderr: `Invalid command line arguments`<br/>
  Код возврата: `1`

[Ссылка на тестирующую систему](https://github.com/spbu-coding-2024/6-grading-system)

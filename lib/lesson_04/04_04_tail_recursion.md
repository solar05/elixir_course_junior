# Хвостовая рекурсия

Надо понимать, что рекурсия -- штука не бесплатная. Каждый рекурсивный вызов требует сохранения на стеке предыдущего состояния функции, чтобы вернуться в него после очередного вызова.

И хотя стек в BEAM легковесный и позволяет делать миллионы рекурсивных вызовов, (а не тысячи, как в большинстве мейнстримовых языков), он, все-таки, конечный.

Поэтому в Эликсир, как и во многих других функциональных языках, компилятор делает одну полезную штуку, которая называется **оптимизация хвостовой рекурсии**.

Если рекурсивный вызов является последней строчкой кода в данной функции, и после него больше никаких инструкций нет, то такой вызов называется хвостовым. В этом случае нет необходимости возвращаться по стеку во все вызывающие
функции, а можно сразу отдать результат из последнего вызова. И стек не растет, а используется заново каждым новым вызовом, и адрес возврата в нем не меняется.

Это позволяет делать бесконечную рекурсию, которая нужна для бесконечно живущих процессов.  А такие процессы нужны серверам :)

Всегда ли нужно использовать хвостовую рекурсию? Мы знаем, что хвостовая рекурсия использует мало памяти для стека. Значит ли это, что нужно всегда использовать ее? Нет. 

Код без хвостовой рекурсии часто получается короче и проще, а иногда и лучше по производительности. Если нам нужна бесконечная, или очень долгая рекурсия (миллионы итераций), то без хвостовой рекурсии не обойтись. Но если мы уверены, что у нас будет конечное и не очень большое число итераций, то можно выбрать вариант без неё.


## Примеры

Давайте посмотрим, как потребляется память в двух реализациях факториала.

Реализация без хвостовой рекурсии:
TODO

We can simulate the memory problem that body-recursive function may face by using a big number to generate millions of recursive calls. You can use a process monitor to see the huge impact it will create in memory. Be ready to kill the process, because it will take a long time to finish. Try it in your 
```
iex> c "factorial.ex
iex> Factorial.of(10_000_000)
```
TODO показать расход памяти.

Реализация с хвостовой рекурсией:
TODO

TODO показать расход памяти.

Мы видим значительное снижение потребления памяти. Но код стал сложнее. Нам пришлось применить подход, который называется "рекурсия с аккумулятором". Это важный паттерн в функциональном программировании, поэтому рассмотрим больше примеров.
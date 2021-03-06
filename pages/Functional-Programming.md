---
layout: page
title: Функционално програмиране в PHP
---

# Функционално програмиране в PHP

PHP поддържа първокласни функции, което означава че функциите могат да бъдат присвоявани на променлива. Както потребителски
така и вградени функции могат да бъдат указвани от променлива и да бъдат извиквани динамично. Функциите предадени като аргументи
на други функции (функции от висок ред) и функции могат да връщат други функции.

Рекурсията (възможността на функция да се самоизвика) се потдържа от езика, но повечето PHP код цели итерация.

Нова конструкция за създаване на безименни (анонимни) функции, които поддържат затваряне, се появява с версия 5.3 (2009).

PHP 5.4 добавя възможността да обвържеш затваряне към областта на видимост (scope) на обекта, както и подобрена поддръжка за
извикваеми обекти, така че да могат да бъдат взаимнозаменими с анонимни функции в повечето случаи.

Най-често срещаното ползване на функции от висок ред е когато се реализира шаблон/образец "Стратегия". Вградената
функция `array_filter` изисква входен масив (данни) и функция (стратегия или функция за обратно извикване), която се
ползва за филтриране на всеки един елемент от масива.

{% highlight php %}
<?php
$input = array(1, 2, 3, 4, 5, 6);

// Създаваме нова безименна функция и я присвояваме на променлива
$filter_even = function($item) {
    return ($item % 2) == 0;
};

// Вградената функция array_filter приема данни и функция за филтриране
$output = array_filter($input, $filter_even);

// Самата функция не е нужно да бъде присвоена на променлива. Това също е валидно:
$output = array_filter($input, function($item) {
    return ($item % 2) == 0;
});

print_r($output);
{% endhighlight %}

Затварянето (closure) е анонимна функция, която има достъп до променливи вмъкнати от външена област на видимост (scope)
без да ползва каквито и да е глобални променливи. Теоретично, затварянето на функция с няколко затворени (т.е. фиксирани)
аргумента от средата когато е била дефинирана. Затварянията могат да заобиколят ограниченията на областа на видимост
по един чист начин.

В следващият пример, ние ще ползваме затваряния за да дефинираме функция, която връща единствена филтрираща функция за
`array_filter` от набор от филтриращи функции.

{% highlight php %}
<?php
/**
 * Създава безименна филтрираща функция приемаща елементи > $min
 *
 * Връща единствен филтър от фамилия филтри "по-големи от n"
 */
function criteria_greater_than($min)
{
    return function($item) use ($min) {
        return $item > $min;
    };
}

$input = array(1, 2, 3, 4, 5, 6);

// Използваме array_filter върху входнни данни с избраната филтърираща фунцкия
$output = array_filter($input, criteria_greater_than(3));

print_r($output); // items > 3
{% endhighlight %}

Всяка едина филтърираща функция от фамилията приема само елементи по-големи от някаква минимална стойност. Филтърът
върнат от `criteria_greater_than` е затваряне с аргумент `$min` затворен по стойност в областта на видимост (предава се като аргумент, когато се извиква `criteria_greater_than`).

Ранно обвързване се ползва по подразбиране за внасянето на променливата `$min` в създадената функция. За истинси затваряния 
с късно обвързване, трябва да се ползват референции когато се внасят. Представете си документни шаблони или библиотека за валидиране на входни данни,
където затварянето е дефинирано да задържи променливи в област на видимост и по-късно да ги достъпи, когато безименната фунцкия се изпълни.

* [Прочети относно безименните функции][anonymous-functions]
* [Повече детайли за затварянията RFC (английски)][closures-rfc]
* [Прочети за динамичното извикване на функции чрез `call_user_func_array`][call-user-func-array]

[anonymous-functions]: http://www.php.net/manual/bg/functions.anonymous.php
[call-user-func-array]: http://php.net/manual/bg/function.call-user-func-array.php
[closures-rfc]: https://wiki.php.net/rfc/closures

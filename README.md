
![Surf iOS Team](https://raw.githubusercontent.com/surfstudio/iOS_Dev/master/img/ios_github.png)

# Surf iOS

В этом репозитории собраны все наши библиотеки, утилиты, инструменты, лучшие практики и форки сторонних библиотек которые мы используем в своей повседневной работе.

**Содержание**
- [Библиотеки](#Библиотеки)
  - [Utils](#utils)
  - [Libs](#libs)
- [Best Practices](#best-practices)
  - [Инициализация проектов](#Инициализация-проектов)
  - [Code style](#code-style)
  - [Архитектура](#Архитектура)
  - [Кодогенерация](#Кодогенерация)
- [Forks](#forks)
- [Правила работы с репозиторием](#Правила-работы-с-репозиторием)

# Библиотеки

Здесь находятся описание и ссылки на наши Pod-библиотеки с открытым исходным кодом. 
Любую из этих библиотек можно установить к себе в проект с помощью `CocoaPods`

## Utils

Содержит набор небольших утилит. 
Все утилиты находятся в одном репозитории, но разбиты по разным `subspecs`

Утилитой может быть форматер телефонных номеров или обертка над `NSAttributedString`

Для получения более подробной информации [iOS Utils](https://github.com/surfstudio/iOS-Utils)

## Libs

Эта секция содержит короткое описание и ссылки на репозитории библиотек которые мы активно разрабатываем, поддерживаем и используем в своих проектах. 

| Название | Описание | Автор | Статус |
| :--- | :--- | :--- | :---: |
| [CoreEvents](https://github.com/surfstudio/CoreEvents) | C#-подобные события | [LastSprint](https://github.com/LastSprint) | [![Build Status](https://travis-ci.org/surfstudio/CoreNetKit.svg?branch=master)](https://travis-ci.org/surfstudio/CoreEvents)
| [CoreNetKit](https://github.com/surfstudio/CoreNetKit) | Позволяет быстро и удобно работать с сетевыми запросами | [LastSprint](https://github.com/LastSprint) | [![Build Status](https://travis-ci.org/surfstudio/CoreNetKit.svg?branch=master)](https://travis-ci.org/surfstudio/CoreNetKit)
| [RDDM](https://github.com/surfstudio/ReactiveDataDisplayManager) | Для удобной работы с UI коллекциями | [LastSprint](https://github.com/LastSprint) | [![Build Status](https://travis-ci.org/surfstudio/ReactiveDataDisplayManager.svg?branch=master&style=flat)](https://travis-ci.org/surfstudio/ReactiveDataDisplayManager)

# Best practices

## Инициализация проектов

- [Xcode-Project-Templates](https://github.com/surfstudio/Xcode-Project-Templates) – набор шаблонов для упрощения процесса создания проекта. Позволяет генерировать необходимые папки, файлы и так далее.

## Code style

- [Swift Code Style](https://github.com/surfstudio/SwiftCodestyle) - указания по оформлению кода на Swift.
- [Obj-C Code Style](https://github.com/surfstudio/objective-c-style-guide) - указания по оформлению кода на Obj-C

## Архитектура

- [Surf MVP](Surf_MVP.md) – наш стандарт разработки UI-слоя приложений

## Кодогенерация

- [Generamba templates](https://github.com/surfstudio/generamba-templates) – содержит набор шаблонов для генерации кода _(шаблон ViewController, шаблон Presenter и т.д.)_ для Generamba

# Forks
| Название | Почему ответвились |
| :--- | :---- |
| [Generamba](github.com/surfstudio/Generamba) | Для работы с Bundler
| [WSTagsField](https://github.com/surfstudio/WSTagsField) |
| [Popover](https://github.com/surfstudio/Popover) | Исправили баг с расчетом размеров Popover'а
| [PluggableApplicationDelegate](https://github.com/surfstudio/PluggableApplicationDelegate)| Поддерживаем |
| [SwiftTheme](https://github.com/surfstudio/SwiftTheme)| Добавили alpha-канал к представлению цвета в hex |
| [MWPhotoBrowser](https://github.com/surfstudio/MWPhotoBrowser)| Багфиксинг и поддержка
| [TLYShyNavBar](https://github.com/surfstudio/TLYShyNavBar) | Поддержка
| [ICViewPager](https://github.com/surfstudio/ICViewPager) | Доработка и поддержка
| [NSObject+Rx](https://github.com/surfstudio/NSObject-Rx) | Добавили совместимость с RxSwift ~> 3.1.0
| [RxGesture](https://github.com/surfstudio/RxGesture) | Понизили deployment target до iOS 8.0
| [OpalImagePicker](https://github.com/surfstudio/OpalImagePicker) | Доработка и поддержка

# Правила работы с репозиторием

Репозиторий создан с целью агрегирования всех собственных библиотек, утилит, инструментов, форков сторонних библиотек и различных практик которые мы используем в своей работе. 

**Для внесения изменений необходимо быть членом iOS команды Surf**

Более детально правила описаны [**здесь**](https://github.com/surfstudio/iOS_Devs/blob/master/CONTRIBUTING.md)

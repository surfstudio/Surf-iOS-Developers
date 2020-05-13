
<img src="img/ios-department-logo.svg" class="site-logo">

# Surf iOS

В этом репозитории собраны все наши библиотеки, утилиты, инструменты, лучшие практики и форки сторонних библиотек которые мы используем в своей повседневной работе.

**Содержание**
- [Лучшие практики](#лучшие-практики)
  - [Инициализация проектов](#инициализация-проектов)
  - [Кодстайл](#кодстайл)
  - [Архитектура](#архитектура)
  - [Кодогенерация](#кодогенерация)
  - [Инструменты](#инструменты)
- [Open Source](#open-source)
  - [Утилиты](#утилиты)
  - [Библиотеки](#библиотеки)
- [Forks](#forks)
- [Правила работы с репозиторием](#правила-работы-с-репозиторием)

# Лучшие практики

## Инициализация проектов

- [Xcode-Project-Templates](https://github.com/surfstudio/Xcode-Project-Templates) – набор шаблонов для упрощения процесса создания проекта. Позволяет генерировать необходимые папки, файлы и так далее.

## Кодстайл

- [Swift Code Style](https://github.com/surfstudio/SwiftCodestyle) - указания по оформлению кода на Swift.
- [Obj-C Code Style](https://github.com/surfstudio/objective-c-style-guide) - указания по оформлению кода на Obj-C

## Архитектура

- [Surf MVP](architectures/Surf_MVP.md) – наш стандарт разработки UI-слоя приложений
- [Surf MVP+Coordinators](architectures/Surf_MVP_Coordinators.md) – надстройка над SurfMVP призванная упростить навигацию внутри приложения. 

## Кодогенерация

- [Generamba templates](https://github.com/surfstudio/generamba-templates) – содержит набор шаблонов для генерации кода _(шаблон ViewController, шаблон Presenter и т.д.)_ для Generamba

## Инструменты

- [TargetsCheck](https://github.com/chausovSurfStudio/TargetsCheck) - скрипт для проверки консистентности проекта, содержащего несколько Targets

# Полезные материалы

* [Материалы для стажеров](usefulMaterials/traineeMaterials.md)
* [Тестовый проект для iOS разработчика](usefulMaterials/testProject.md)
* [Проекты для iOS стажеров](https://github.com/surfstudio/iOSSpringSchool2020/blob/master/practice.md#%D1%82%D0%B5%D0%BC%D1%8B)

# Open Source

Здесь находятся описание и ссылки на наши Pod-библиотеки с открытым исходным кодом. 
Любую из этих библиотек можно установить к себе в проект с помощью `CocoaPods`

## Утилиты

Содержит набор небольших утилит. 
Все утилиты находятся в одном репозитории, но разбиты по разным `subspecs`

Утилитой может быть форматер телефонных номеров или обертка над `NSAttributedString`

Для получения более подробной информации [iOS Utils](https://github.com/surfstudio/iOS-Utils)

## Библиотеки

Эта секция содержит короткое описание и ссылки на репозитории библиотек которые мы активно разрабатываем, поддерживаем и используем в своих проектах. 

[Как добавить свою библиотеку](/ADD_NEW_LIB_TUTORIAL.md)

| Название | Описание | Автор | Статус |
| :--- | :--- | :--- | :---: |
| [CoreEvents](https://github.com/surfstudio/CoreEvents) | C#-подобные события | [LastSprint](https://github.com/LastSprint) | [![Build Status](https://travis-ci.org/surfstudio/CoreNetKit.svg?branch=master)](https://travis-ci.org/surfstudio/CoreEvents)
| [NodeKit](https://github.com/surfstudio/NodeKit) | Позволяет быстро и удобно работать с сетевыми запросами | [LastSprint](https://github.com/LastSprint) | [![Build Status](https://travis-ci.org/surfstudio/NodeKit.svg?branch=master)](https://travis-ci.org/surfstudio/NodeKit)
| [RDDM](https://github.com/surfstudio/ReactiveDataDisplayManager) | Для удобной работы с UI коллекциями | [LastSprint](https://github.com/LastSprint) | [![Build Status](https://travis-ci.org/surfstudio/ReactiveDataDisplayManager.svg?branch=master&style=flat)](https://travis-ci.org/surfstudio/ReactiveDataDisplayManager)
| [TextFieldsCatalog](https://github.com/chausovSurfStudio/TextFieldsCatalog) | Коллекция богатых и хорошо кастомизируемых текстовых полей | [chausovSurfStudio](https://github.com/chausovSurfStudio) | [![Build Status](https://travis-ci.org/chausovSurfStudio/TextFieldsCatalog.svg?branch=master&style=flat)](https://travis-ci.org/chausovSurfStudio/TextFieldsCatalog)
| [MaskInterpreter](https://github.com/surfstudio/MaskInterpreter) | Интерпритатор масок для пользовательского ввода | [LastSprint](https://github.com/LastSprint) | [![Actions Status](https://github.com/LastSprint/MaskInterpreter/workflows/CI/badge.svg)](https://github.com/LastSprint/MaskInterpreter/actions)
| [OTPTextField](https://github.com/fixique/OTPTextField) | Библиотека для реализации OTP поля ввода | [Fixique](https://github.com/fixique) | [![Build Status](https://travis-ci.com/fixique/OTPTextField.svg?branch=master)](https://travis-ci.com/fixique/OTPTextField) 

# Forks

| Название | Почему ответвились |
| :--- | :---- |
| [Generamba](github.com/surfstudio/Generamba) | Для работы с Bundler
| [WSTagsField](https://github.com/surfstudio/WSTagsField) | Исправили краш и поддержка
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


![Surf iOS Team](https://raw.githubusercontent.com/surfstudio/iOS_Dev/master/img/surf_logo.png)
# Surf iOS

Репозиторий содержит ссылки и мета информацию о библиотеках, утилитах, подходах и т.п.

**Содержание**

- [Pods](#pods)
  - [Utils](#utils)
  - [Libs](#libs)
  - [Forks](#forks)
- [Best Practices](#best-practices)
  - [Инициаллизация проектов](#Инициаллизация проектов)
  - [Code style](#code-style)
  - [Кодогенерация](#кодогенерация)
 
# Pods

Содержит описание и ссылки на библиотеки с открытым исходным кодом. 
Любую из этих библиотек можно установить к себе в проект с помощью `CocoaPods`

## Utils

Содержит набор небольших утилит. 
Все утилиты находятся в однмо репозитории, но разбиты по разным `subspecs`

Утилитой может быть форматер телефонных номеров или обертка над `NSAttributedString`

Для получения более подробной информации [iOS Utils](https://github.com/surfstudio/iOS-Utils)

## Libs

Эта секция содержит короткое описание и ссылки на репозитории библиотек которые мы активно разрабатываем, поддерживаем и используем в своих проектах. 

| Название | Описание | Автор | Статус |
| :--- | :--- | :--- | :---: |
| [CoreEvents](https://github.com/surfstudio/CoreEvents) | C#-подобные события | [LastSprint](https://github.com/LastSprint) | [![Build Status](https://travis-ci.org/surfstudio/CoreNetKit.svg?branch=master)](https://travis-ci.org/surfstudio/CoreEvents)
| [CoreNetKit](https://github.com/surfstudio/CoreNetKit) | Позволяет быстро и удобно работать с сетевыми запросами | [LastSprint](https://github.com/LastSprint) | [![Build Status](https://travis-ci.org/surfstudio/CoreNetKit.svg?branch=master)](https://travis-ci.org/surfstudio/CoreNetKit)
| [RDDM](https://github.com/surfstudio/ReactiveDataDisplayManager) | Для удобной работы с UI коллекциями | [LastSprint](https://github.com/LastSprint) | [![Build Status](https://travis-ci.org/surfstudio/ReactiveDataDisplayManager.svg?branch=master&style=flat)](https://travis-ci.org/surfstudio/ReactiveDataDisplayManager)

**Deprecated**

| Название | Описание | Автор |
| :--- | :--- | :--- |
| [CSL](https://bitbucket.org/surfstudio/cslswift/src/master/) | Упрощает сетевое взаимодействие (Obj-C) | Surf
| [CSLSwift](https://bitbucket.org/surfstudio/cacheableservicelayer/src) | Упрощает сетевое взаимодействие (Swift) | Surf 
| [SurfObjcUtils](https://bitbucket.org/surfstudio/surfobjcutils/src/master/) | Коллекция утилит на Obj-C | Surf 

## Forks
| Название | Почему ответвились |
| :--- | :---- |
| [Generamba](github.com/surfstudio/Generamba) | Для работы с Bundler

# Best practices

## Инициаллизация проектов
- [Xcode-Project-Templates](https://github.com/surfstudio/Xcode-Project-Templates) набор шаблонов для упрощения процесса создания проекта. Позваоляет генерировать необходимые папки, файлы и так далее.

## Code style
- [Swift Code Style](https://github.com/surfstudio/SwiftCodestyle) - указания по оформлению кода на Swift.
- [Obj-C Code Style](https://github.com/surfstudio/objective-c-style-guide) - указания по оформлению кода на Obj-C

## Кодогенерация
- [Generamba templates](https://github.com/surfstudio/generamba-templates) содержит набор шаблонов для генерации кода _(шаблон ViewController, шаблон Presenter и т.д.)_ для Generamba

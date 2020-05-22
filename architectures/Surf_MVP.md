# Surf MVP

## Содержание

- [Предисловие](#предисловие)
- [Описание архитектуры](#описание-архитектуры)
  - [Взаимодействие между слоями](#взаимодействие-между-слоями)
    - [View](#view)
        - [ViewInput](#viewinput)
        - [ViewOutput](#viewoutput)
		- [Правила наименования методов ViewInput и ViewOutput](#правила-наименования-методов-viewinput-и-viewoutput)
        - [ModuleTransitionable](#moduletransitionable)
            - [Реализация кастомного перехода](#реализация-кастомного-перехода)
    - [Presenter](#presenter)
        - [ModuleInput](#moduleinput)
        - [ModuleOutput](#moduleoutput)
    - [Router](#router)
        - [RouterInput](#routerinput)
		- [Правила наименования методов в протоколе RouterInput](#правила-наименования-методов-в-протоколе-routerinput)
    - [Configurator](#configurator)
- [Лучшие практики](#лучшие-практики)
  - [Работа с коллекциями](#работа-с-коллекциями)
    - [Адаптеры](#адаптеры)
    - [ReactiveDataDisplayManger](#reactivedatadisplaymanager)
  - [Взаимодействие с UIAlertController](#взаимодействие-с-uialertcontroller)
- [Кодогенерация](#кодогенерация)
- [Внедрение Surf MVP](#внедрение-surf-mvp)
  - [Surf MVP в существующем проекте](#surf-mvp-в-существующем-проекте)
  - [Surf MVP в новом проекте](#surf-mvp-в-новом-проекте)

# Предисловие

При разработке и поддержке большого количества приложений возникает много сложностей. Они связаны с организацией кодовой базы и её обменом между приложениями.

Если на каждом проекте свои стандарты, при переключении между проектами разработчики неминуемо ошибаются, особенно в стиле написания кода.

Мы ввели стандартизированную архитектуру с прописанным набором правил и взаимодействий. Она поможет разработчикам создавать грамотный и структурированный код в проектах, а также легко переключаться между ними.

Шаблон **MVP** — наш стандарт разработки UI-слоя приложений. Он решает следующие проблемы создания и поддержки качественных продуктов:
* **Тестируемость** — в MVP бизнес-логика отделена от ViewController и закрыта протоколами, поэтому она отлично покрывается тестами.

* **Переиспользуемость** — в MVP модули обособлены друг от друга. Их можно легко переиспользовать внутри одного и в разных приложениях.

* **Разделение обязанностей** — в MVP все обязанности четко расписаны. Каждый компонент модуля отвечает за конкретную работу. Это позволяет сократить количество строк кода на класс.

### Слой бизнес-логики

Для реализации бизнес-логики используется сервис-ориентированную архитектуру **SOA** (Service-Oriented-Architecture). Вот какие проблемы она решает:

* **Тестируемость** — SOA хорошо подходит для unit-тестирования, потому что сервис разбит на отдельные слои. Каждый из них отвечает за определенную функцию. Благодаря этому его можно протестировать отдельно. SOA выполняет работу с core-компонентами. Теперь их не нужно тестировать.

* **Переиспользуемость** — в сервисах содержатся части бизнес-логики. Их можно легко встроить в разные приложения.

* **Разделение обязанностей** — сервисы четко разделяют обязанности доступа к сети, к базе из промежуточных слоев.

Все подходы к архитектуре основаны на идеях Clean Architecture и SOLID  Роберта Мартина.

# Описание архитектуры

В основе архитектуры SurfMVP лежит классический MVP (Model View Presenter) — шаблон проектирования, который используется для построения пользовательского интерфейса.

![surf_mvp_old](../img/SurfMVP/surf_mvp_old.jpeg)

<p align="center">Схема классического MVP - модуля</p>

**View** – отображает данные на экране и оповещает **Presenter** о действиях пользователя. Пассивна — **View** никогда не запрашивает данные, только получает их от **Presenter**.

**Presenter** – получает от **View** информацию о действиях пользователя и реагирует на них. Передает события в **Model** для обновления или обработки внутри себя. Ничего не должен знать о UIKit, за исключением UIImage.

**Model** – заключает в себе всю бизнес-логику, необходимую для работы модуля.

#### Что мы добавили?

* Теперь за сборку отдельного модуля отвечает сущность **Configurator**, она инициализирует все необходимые компоненты и отвечает за простановку зависимостей между ними. 

* Мы заметили некоторые проблемы при переходах между экранами в iOS приложениях. Большое количество логики по формированию новых экранов перед переходом сосредотачивается непосредственно во UIViewController, нам показалось это не совсем правильным, поэтому первым делом мы выделили отдельную сущность **Router**, которая отвечает за осуществление переходов между экранами в приложении. 

* **Model** в SurfMVP – это сервисы, которые вызывает Presenter для получения данных. Зачастую, один сервис решает задачи для работы целого модуля, однако в сложных ситуациях приходится взаимодействовать с несколькими. 

![surf_mvp_new](../img/SurfMVP/surf_mvp_new.jpeg)

<p align="center">Схема Surf MVP - модуля</p>

### Взаимодействие между слоями

Основная особенность **SurfMVP**  —  каждый слой в MVP отделен от другого протоколом. На изображении видно схему слоев и связь протоколов между ними. Протоколы нужны, чтобы каждый слой был обособлен от другого и в теории легко заменялся. Каждый из слоев не должен раскрывать детали реализации. 

![surf_mvp_layers](../img/SurfMVP/surf_mvp_layers.jpeg)
<p align="center">Схема слоев SurfMVP</p>

**Рассмотрим их отдельно:** 

## View

**View** заключает в себе логику отображения и заполнения себя данными. Передает в **Presenter** все пользовательские действия.



### ViewInput

**ViewInput** – реализует сама **View**, ссылку держит **Presenter**. Данный протокол описывает методы, при помощи которых **Presenter** может управлять **View**, передавать данные, изменять состояния и так далее. 

Пример **ViewInput** некоторой view, которая отображает профиль пользователя и его абонемент.

```swift
protocol ProfileViewInput: class {

    /// Method for setup initial state of view
    func setupInitialState()

    /// Method for fill view fields with UserProfile
    func configure(with profile: UserProfile)

    /// Method for fill view fields with Subscription
    func configure(with subscription: Subscription)

}
```

> Методы конфигурирования View с помощью какого-то параметра лучше называть, как в примере выше. Это необходимо для стандартизации.



### ViewOutput

**ViewOutput** – реализует **Presenter**, ссылку на него держит **View**. Протокол описывает набор действий, которые могут произойти во **View**, и методы жизненного цикла, например, события взаимодействия пользователя с экраном.

Пример **ViewOutput** уже знакомой нам view — на ней пользователь может перезагрузить данные или отредактировать профиль.

```swift
protocol ProfileViewOutput: class {

    /// Notify that view is ready
    func viewLoaded()

    // Notify that need reload view data
    func reload()

    // Notify that need edit profile
    func editProfile()

}
```

<details>
<summary>Правила наименования методов ViewInput и ViewOutput</summary>

В протоколах *ViewInput* и *ViewOutput* многие ошибаются в наименовании методов, раскрывая дeтали реализации **View**.

**Плохой пример:**

```swift
func loginButtonClick()
```

**Хороший пример:**

```swift
func login()
```

> Мы передаем пользовательское намерение, не завязываясь на дeтали реализации **View**. Если кнопка поменяется на ячейку таблицы, протокол не нужно будет трогать.*  

**Ещё плохой пример:**

```
func reloadTable()
```

**Хороший пример:**
```swift
func reload()
```

> Стараемся не завязываться на детали реализации View. В любой момент сможем поменять таблицу на Collection View, не трогая протокол.  

**И еще один плохой пример:**

```swift
func configureTableViewAdapter(with: SomeParameter)
```

**Хороший пример:**

```swift
func configure(with: SomeParameter)
```

> Мы не раскрываем реализацию view **Presenter**-у. Он не знает, как устроена **View**. Он видит набор методов взаимодействия. Вводя метод *configureTableViewAdapter*, мы говорили ему, что View содержит таблицу.

</details>


### ModuleTransitionable

**ModuleTransitionable** – данный протокол реализуется **View**, ссылку на него держит **Router**. Это единственный «базовый» протокол в **SurfMVP**. Он нужен для того, чтобы предоставить Router набор методов для работы с навигацией по приложению. 

Реализацию можно посмотреть в [шаблонах проектов.](https://github.com/surfstudio/Xcode-Project-Templates/blob/master/Surf%20MVP%20Application.xctemplate/Support/ModuleTrasitionable.swift) **Рекомендуется инициализировать проект из шаблона, а не копировать файл.**

Так как у протокола есть базовая реализация — все базовые методы отображения не нужно реализовывать каждый раз.

###### Реализация кастомного перехода

Если необходимо создать кастомный переход между модулями:

* Создать свой протокол **CustomModuleTransitionable**
* Заимплементить им протокол **ModuleTransitionable**
* Реализовать **View**
* Не забыть поменять ссылку в **Router**

Пример реализации кастомного перехода:

```swift
protocol CustomModuleTransitionable: class, ModuleTransitionable {
    func showModuleWithCustomTransition(_ module: UIViewController)
}

extension CustomModuleTransitionable  where Self: UIViewController {
    func showModuleWithCustomTransition(_ module: UIViewController) {
        // do something to show B
    }
}
```



## Presenter

**Presenter** — управляющий элемент модуля. Он получает данные из **Model** и преобразует их в необходимый вид. Потом отдает во **View** для отображения.
**Presenter** контролирует, когда и какое состояние **View** нужно отобразить. Если у **View** два состояния, с данными и без, **Presenter** определит, когда и какое состояние отобразить.
**Presenter** решает, как реагировать на пользовательские действия.

### ModuleInput

**ModuleInput** – реализует **Presenter**. Данный протокол должен содержать в себе методы, при помощи которых другой модуль, который держит ссылку на этот протокол, мог бы изменять состояния текущего модуля.

Пример **ModuleInput** модуля профиля. С ним можно передать загруженный профиль пользователя во время открытия модуля.
```swift
protocol ProfileModuleInput: class {
    /// Method for configure module with UserProfile entity.
    func configureModule(with profile: UserProfile)
}
```

> Методы конфигурирования модуля с помощью какого-то параметра лучше называть, как в примере выше.  

### ModuleOutput 

**ModuleOutput** – реализует **Presenter** вызывающего модуля, ссылку держит **Presenter** вызываемого модуля. Если экран профиля можно отобразить с модуля новостей, то NewsPresenter должен реализовывать ProfileModuleOutput, а ProfilePresenter содержать на него ссылку. **ModuleOutput** передается в Configurator вызываемого модуля и там устанавливается в **Presenter**. Содержит в себе методы модуля, которые влияют на поведение вызывающего модуля.

Пример **ModuleOutput** модуля профиля, при помощи которого можно сообщать об изменении профиля вызывающему модулю.
```swift
protocol ProfileModuleOutput: class {
    /// Notify that user profile edited
    func profileEdited()
}
```



## Router

**Router** отвечает за конфигурацию и отображение других модулей. Под другими модулями не обязательно подразумевается *ViewController*. Это может быть дочерняя *UIView*, какое-либо всплывающее сообщение об ошибке и т.п.

### RouterInput 

**RouterInput** – этот протокол реализует **Router,** а ссылку на него держит **Presenter,** так как он является единственным ответственным за то, чтобы инициировать дальнейшую навигацию в приложении.

Пример **RouterInput** модуля профиля. С ним можно показать модуль редактирования профиля.

```swift
protocol ProfileRouterInput {
	  /// Method for transition to profile module
    func showEditProfileModule()
}
```


<details>
<summary>Правила наименования методов в протоколе RouterInput</summary>

**Замечание**
В протоколе **RouterInput** часто возникают ошибки наименования методов. В имени раскрываются детали реализации модуля. Называть нужно, не привязываясь к деталям реализации.

**Плохой пример:**

```swift
func showEditProfileScreen()
```

**Хороший пример:**

```swift
func showEditProfileModule()
```

> Мы просим показать модуль, а не экран. Благодаря этому, если нужно показать алерт, не придется менять протокол.  

**Ещё плохой пример:**

```swift
func showConfirmationAlert()
```

**Хороший пример:**

```swift
func showConfirmationModule()
```

> Мы стараемся не завязываться на детали реализации отображаемого модуля. Таким образом, мы в любой момент сможем поменять алерт на модальное окно, не трогая  протокол.  



</details>

## Configurator

У **Configurator** нет протоколов. Он содержит только набор методов конфигурации модуля с разными входными данными.

Пример **Configurator** вышеописанного модуля профиля.

```swift
final class ProfileModuleConfigurator {

    // MARK: Internal methods

    func configure(with profile: UserProfile) -> ProfileViewController {
        let view = ProfileViewController.controller()
        let presenter = ProfilePresenter(with: profile)
        let router = ProfileRouter()

        presenter.view = view
        presenter.router = router
        router.view = view
        view.output = presenter

        return view
    }

}
```



# Лучшие практики

## Работа с коллекциями
### Адаптеры
Во многих приложениях более половины экранов — коллекции *UITableView* или *UICollectionView*. Важно выбрать правильный подход разработки данных элементов, чтобы не проиграть в скорости и поддержке готовых решений.

Из-за того, что *UITableViewDelegate* и *UITableViewDataSource* (или UICollectionViewDelegate и UICollectionViewDataSource) реализованные **ViewController-ом** не соответствует [принципу единственной ответственности](https://ru.wikipedia.org/wiki/Принцип_единственной_ответственности). Для того, чтобы решить эту проблему был выделен отдельный объект **Adapter**, который реализует эти протоколы.  

**Adapter** избавляет **UIViewController** от знания о внутреннем устройстве коллекции.
**Adapter** создается и хранится во **View**. **View** получает необходимые данные. Далее она передает в **Adapter** ссылку на коллекцию для регистрации ячеек и информацию для запуска коллекции.

Для передачи действий из ячеек и при нажатии на них есть два разных подхода: 

* Создается отдельный протокол **AdapterOutput** 
* Используются closure или [Events](https://github.com/surfstudio/CoreEvents)

> Используемый подход разнится в зависимости от проекта. Следует уточнять у лида проекта.


Пример output-a для модуля профиля.

```swift
protocol ProfileViewAdapterOutput {
    func didSelectProfileView()
}
```

Сниппет для быстрого создания табличных адаптеров находится в [репозитории со сниппетами.](https://github.com/ismetanin/XcodeCodeSnippets)

Мы закрываем **View** от знания о коллекции. Остается только IBOutlet и передача ее в адаптер. Так мы можем в любой момент написать другой адаптер и менять отображение в зависимости от условий.

Конфигурацию ячеек нужно производить внутри самих ячеек. Разберем на примере:

```swift
guard let cell = tableView.dequeueReusableCell(withIdentifier: ProfileCell.nameOfClass, for: indexPath) as? ProfileCell else {
    return UITableViewCell()
}
cell.configure(with: subscription)
return cell
```

Мы не открываем **Adapter** внутреннее устройство ячейки. Только предоставляем набор методов для ее конфигурации.

> **Внимание!** данный метод постепенно заменяется на RDDM на всех проектах. Однако не исключается возможность комбинировать Adapter+RDDM.

### ReactiveDataDisplayManager

В большом количестве проектов студии используется имеено этот подход с RDDM. 

Библиотека [ReactiveDataDisplayManger](https://github.com/LastSprint/ReactiveDataDisplayManager) предоставляет интерфейс **DDM** (Data Display Manager). Используя данный подход можно конфигурировать коллекции по элементам при помощи генераторов ячеек.
Это удобно для коллекций с разнородными ячейками. Можно последовательно передать блоки в **DDM** и не писать огромный switch-case.
Более подробно о подходе в [репозитории проекта](https://github.com/LastSprint/ReactiveDataDisplayManager).

## Взаимодействие с UIAlertController

Очень часто разработчики сталкиваются с вопросом отображения **UIAlertController**. Не понятно, какой компонент показывает алерт, кто его конфигурирует и т.п.

Так как алерт является отдельным контроллером, то и показывать его стоит как отдельный модуль. В зависимости от логики алертов, выделяется несколько способов их конфигурирования:

* **UIAlertController без внутренней логики**
Если алерт однокнопочный, т.е. просто отображает какое-то сообщение, его следует конфигурировать прямо в **Router**.

Пример отображения и конфигурирования алерта в **Router**
```swift
SomeModuleRouter.swift

func showMessageModule(with message: String) {
    let alertController = UIAlertController(title: nil, message: message, preferredStyle: .alert)
    let cancelAction = UIAlertAction(title: nil, style: .cancel, handler: nil)
    alertController.addAction(cancelAction)
    self.present(alertController, animated: true, completion: nil)
}
```

* **UIAlertController с логикой**
Если алерт содержит какую-либо логику, реакцию на действия, его следует оборачивать в отдельный модуль и отображать через **Router**. Реакцию на действия нужно производить через *ModuleOutput* данного алерта.


Пример отображения и конфигурирования алерта через **Router**
```swift
SomeModuleRouter.swift

func showActionsModule(with bankCard: BankCard, output: ActionsWithBankCardModuleOutput) {
        let alertController = ActionsWithBankCardAlertViewController(title: nil, message: nil, preferredStyle: .actionSheet)
        alertController.configure(card: bankCard, output: output)
        view?.present(alertController, animated: true, completion: nil)
}
```

```swift
ActionsWithBankCardAlertViewController.swift

protocol ActionsWithBankCardModuleOutput {
    func remove(bankCard: BankCard)
}

final class ActionsWithBankCardAlertViewController: UIAlertController {

    // MARK: - Properties

    private var output: ActionsWithBankCardModuleOutput!
    private var bankCard: BankCard!

    // MARK: - UIAlertController

    override func viewDidLoad() {
        super.viewDidLoad()
        let removeAction = UIAlertAction(title: L10n.unbind, style: .default) { [weak self] _ in
            guard let `self` = self else {
                return
            }
            self.output?.remove(bankCard: self.bankCard)
        }
        self.addAction(removeAction)
        self.view.tintColor = UIColor(named: .MainTheme)
        let cancelAction = UIAlertAction(title: L10n.cancel, style: .cancel, handler: nil)
        self.addAction(cancelAction)
    }

    // MARK: - Internal methods

    func configure(card: BankCard, output: ActionsWithBankCardModuleOutput) {
        self.bankCard = card
        self.output = output
    }
}
```



# Кодогенерация

Создание нового модуля достаточно трудоемкая задача. Для того, чтобы это сделать, нужно:
* Четыре новых класса (*Configurator*, *Router*, *Presenter*, *ViewController*)
* Пять новых протоколов (*ViewInput*, *ViewOutput*,  *RouterInput*, *ModuleInput*, *ModuleOutput*)
* Четыре новых теста (*ConfiguratorTests*, *RouterTests*, *PresenterTests*, *ViewControllerTests*)

Прописать все зависимости в **Confugrator**, реализовать протоколы. Это только в стандартном случае. Для того, чтобы избежать утомительной механической работы, можно воспользоваться утилитой для кодогенерации – [Generamba](https://github.com/strongself/Generamba).

Для более детального знакомства с инструментом рекомендуется прочитать документацию в репозитории, а также [вводную статью на хабре](https://habrahabr.ru/company/rambler-co/blog/276275/).

Для генерации Surf MVP модулей есть готовый [шаблон](https://github.com/surfstudio/generamba-templates). 
Достаточно правильно настроить Rambafile.
Ниже — пример Rambafile

```ruby
### Headers settings
company: Surf

### Xcode project settings
project_name: YourProjectName
xcodeproj_path: YourProjectName.xcodeproj

### Code generation settings section
# The main project target name
project_target: YourProjectName

# The file path for new modules
project_file_path: YourProjectName/Screens

# The Xcode group path to new modules
project_group_path: YourProjectName/Screens

### Tests generation settings section
# The tests target name
test_target: YourProjectNameTests

# The file path for new tests
test_file_path: YourProjectNameTests/Tests/Screens

# The Xcode group path to new tests
test_group_path: YourProjectNameTests/Tests/Screens

### Dependencies settings section
podfile_path: Podfile

### Catalogs
catalogs:
- 'https://github.com/surfstudio/generamba-templates'

### Templates
templates:
- {name: surf_mvp_module}
```

Теперь для генерации модуля достаточно перейти в Terminal
в папку проекта и прописать` generamba gen YourModuleName surf_mvp_module.`

### Внедрение Surf MVP

#### Surf MVP в существующем проекте

Допустим, у вас огромное приложение, написанное на Cocoa-MVC с огромными контроллерами.
Каждый Surf MVP модуль — это самостоятельная единица. Можно начать писать новые экраны в проекте на Surf MVP.

Для интеграции Surf MVP в существующий проект достаточно:
* Прочитать всей команде эту прекрасную статью
* Добавить базовый протокол ModuleTransitionable для **View** (чтобы работали переходы между модулями) 
* Подключить Генерамбу, чтобы не создавать все модули руками.

Теперь смело создавайте Surf MVP модули и пользуйтесь ими.

#### Surf MVP в новом проекте

Подход создания ничем не отличается от внедрения в уже существующий. В качестве структуры проекта используйте [Xcode-шаблон](https://github.com/surfstudio/Xcode-Project-Templates).

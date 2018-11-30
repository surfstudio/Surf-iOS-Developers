# Surf MVP

**Содержание**
- [Предисловие](#Предисловие)
- [Описание архитектуры](#Описание-архитектуры)
  - [Описание](#Описание)
  - [Взаимодействие между слоями](#Взаимодействие-между-слоями)
    - [View](#View)
        - [ViewInput](#ViewInput)
        - [ViewOutput](#ViewOutput)
        - [ModuleTransitionable](#ModuleTransitionable)
            - [Реализация кастомного перехода](#Реализация-кастомного-перехода)
    - [Presenter](#Presenter)
        - [ModuleInput](#ModuleInput)
        - [ModuleOutput](#ModuleOutput)
    - [Router](#Router)
        - [RouterInput](#RouterInput)
    - [Configurator](#Configurator)
- [Лучшие практики](#Лучшие-практики)
  - [Работа с коллекциями](#Работа-с-коллекциями)
    - [Адаптеры](#Адаптеры)
    - [ReactiveDataDisplayManger](#ReactiveDataDisplayManager)
  - [Взаимодействие с UIAlertController](#Взаимодействие-с-UIAlertController)
  - [Кодогенерация](#Кодогенерация)
- [Внедрение Surf MVP](#Внедрение-Surf-MVP)
  - [Surf MVP в существующем проекте](#Surf-MVP-в-существующем-проекте)
  - [Surf MVP в новом проекте](#Surf-MVP-в-новом-проекте)

## Предисловие

При разработке и поддержке большого количества приложений возникает много сложностей. Они связаны с организацией кодовой базы и её обменом между приложениями.

Если на каждом проекте свои стандарты, при переключении между проектами разработчики неминуемо ошибаются, особенно в стиле написания кода.

Мы ввели стандартизированную архитектуру с прописанным набором правил и взаимодействий. Она поможет разработчикам создавать грамотный и структурированный код в проектах, а также легко переключаться между ними.

Шаблон **MVP** — наш стандарт разработки UI-слоя приложений. Он решает следующие проблемы создания и поддержки качественных продуктов:
* **Тестируемость** — в MVP бизнес-логика отделена от ViewController и закрыта протоколами, поэтому она отлично покрывается тестами.
* **Переиспользуемость** — в MVP модули обособлены друг от друга. Их можно легко переиспользовать внутри одного и в разных приложениях.
* **Разделение обязанностей** — в MVP все обязанности четко расписаны. Каждый компонент модуля отвечает за конкретную работу. Это позволяет сократить количество строк кода на класс.

	Мы использовали сервис-ориентированную архитектуру **SOA** (Service-Oriented-Architecture) для реализации слоя бизнес-логики. Вот какие проблемы она решает:
* **Тестируемость** — SOA хорошо подходит для unit-тестирования, потому что сервис разбит на отдельные слои. Каждый из них отвечает за определенную функцию. Благодаря этому его можно протестировать отдельно. SOA выполняет работу с core-компонентами. Теперь их не нужно тестировать.
* **Переиспользуемость** — в сервисах содержатся части бизнес-логики. Их можно легко встроить в разные приложения.
* **Разделение обязанностей** — сервисы четко разделяют обязанности доступа к сети, к базе из промежуточных слоев.

Все подходы к архитектуре основаны на идеях Clean Architecture и SOLID  Роберта Мартина.

## Описание архитектуры

### Описание

MVP (Model View Presenter) — шаблон проектирования, который используется для построения пользовательского интерфейса.

![surf_mvp_old](https://github.com/surfstudio/Surf-iOS-Developers/blob/surf_mvp/img/surf_mvp_old.png)

<p align="center">Схема классического MVP - модуля</p>

**View**: отображает данные на экране и оповещает Presenter о действиях пользователя. Пассивен — сам никогда не запрашивает данные, только получает их от Presenter.

**Presenter**: получает от **View** информацию о действиях пользователя и реагирует на них. Передает события в модель для обновления или обработки внутри себя. Ничего не должен знать о UIKit, за исключением UIImage.

**Model**: заключает в себе всю бизнес-логику, необходимую для работы модуля.

Мы заметили проблемы при переходе между модулями в iOS-приложениях. В Surf MVP выделили сущность **Router**, которая осуществляет переходы между модулями.
Для конфигурирования модулей выделили сущность **Configurator**. Она отвечает за сборку модуля и проставление зависимостей между компонентами.
**Model** в Surf MVP — это сервисы, которые вызывает **Presenter** для получения данных. Зачастую один сервис решает задачи для работы целого модуля. В сложных случаях приходится взаимодействовать с несколькими.

![surf_mvp_new](https://github.com/surfstudio/Surf-iOS-Developers/blob/surf_mvp/img/surf_mvp_new.png)

<p align="center">Схема Surf MVP - модуля</p>

### Взаимодействие между слоями

Каждый слой в MVP отделен от другого протоколом. Ниже — схема слоев и протоколов между ними. Мы отдельно описали каждый протокол.

![surf_mvp_layers](https://github.com/surfstudio/Surf-iOS-Developers/blob/surf_mvp/img/surf_mvp_layers.png)

#### View

**View** заключает в себе логику отображения и заполнения себя данными. Передает в **Presenter** все пользовательские действия.

##### ViewInput

– реализуется непосредственно **View**, ссылку держит **Presenter**.
Описывает методы, при помощи которых **Presenter** управляет отображением.

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

##### ViewOutput

– реализуется **Presenter**, ссылку держит **View**. 
Описывает набор действий, которые могут произойти во **View** и методы жизненного цикла для оповещения **Presenter**.

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

**Замечание**
В протоколах *ViewInput* и *ViewOutput* многие ошибаются в наименовании методов, раскрывая дeтали реализации **View**.

**Плохой пример:**

```
func loginButtonClick()
```

**Хороший пример:**

```
func login()
```

> Мы передаем пользовательское намерение, не завязываясь на дeтали реализации **View**. Если кнопка поменяется на ячейку таблицы, протокол не нужно будет трогать.*  

**Ещё плохой пример:**

```
func reloadTable()
```

**Хороший пример:**
```
func reload()
```

> Стараемся не завязываться на детали реализации View. В любой момент сможем поменять таблицу на Collection View, не трогая протокол.  

**И еще один плохой пример:**

```
func configureTableViewAdapter(with: SomeParameter)
```

**Хороший пример:**

```
func configure(with: SomeParameter)
```

> Мы не раскрываем реализацию view **Presenter**-у. Он не знает, как устроена **View**. Он видит набор методов взаимодействия. Вводя метод *configureTableViewAdapter*, мы говорили ему, что View содержит таблицу.  

##### ModuleTransitionable

– реализуется **View**, ссылку держит **Router**. Это единственный “базовый” протокол в Surf MVP. Он нужен для того, чтобы предоставить **Router** набор методов для работы с навигацией по приложению.

```swift
protocol ModuleTransitionable: class {
    func showModule(_ module: UIViewController)
    func dismissView(animated: Bool, completion: (() -> Void)?)
    func presentModule(_ module: UIViewController, animated: Bool, completion: (() -> Void)?)
    func pop(animated: Bool)
    func push(module: UIViewController, animated: Bool)
}

extension ModuleTransitionable where Self: UIViewController {

    func showModule(_ module: UIViewController) {
        self.show(module, sender: nil)
    }

    func dismissView(animated: Bool, completion: (() -> Void)?) {
        self.presentingViewController?.dismiss(animated: animated, completion: completion)
    }

    func presentModule(_ module: UIViewController, animated: Bool, completion: (() -> Void)?) {
        self.present(module, animated: animated, completion: completion)
    }

    func pop(animated: Bool) {
        self.navigationController?.popViewController(animated: animated)
    }

    func push(module: UIViewController, animated: Bool) {
        self.navigationController?.pushViewController(module, animated: animated)
    }
}
```

Так как у протокола есть базовая реализация — все базовые методы отображения не нужно выполнять каждый раз.

###### Реализация кастомного перехода

Если необходимо создать кастомный переход между модулями:
1) Создать свой протокол **CustomModuleTransitionable**
2) Заимплементить им протокол **ModuleTransitionable**
3) Реализовать **View**
4) Не забыть поменять ссылку в **Router**

Пример реализации кастомного перехода

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

#### Presenter

**Presenter** — управляющий элемент модуля. Он получает данные из **Model** и преобразует их в необходимый вид. Потом отдает во **View** для отображения.
**Presenter** контролирует, когда и какое состояние **View** нужно отобразить. Если у **View** два состояния, с данными и без, **Presenter** определит, когда и какое состояние отобразить.
**Presenter** решает, как реагировать на пользовательские действия.

##### ModuleInput 

– реализуется **Presenter**. Описывает методы, при помощи которых сконфигурировать начальный стейт модуля. Вызывается в **Configurator**. **ModuleInput** содержит набор методов, принимающих некоторые параметры. Они потом хранятся в **Presenter**.

Пример **ModuleInput** модуля профиля. С ним можно передать загруженный профиль пользователя во время открытия модуля.
```swift
protocol ProfileModuleInput: class {
    /// Method for configure module with UserProfile entity.
    func configureModule(with profile: UserProfile)
}
```

> Методы конфигурирования модуля с помощью какого-то параметра лучше называть, как в примере выше.  

##### ModuleOutput 

– реализуется **Presenter** вызывающего модуля, ссылку держит **Presenter** вызываемого модуля. Если экран профиля можно отобразить с модуля новостей, то *NewsPresenter* должен реализовывать *ProfileModuleOutput*, а *ProfilePresenter* — содержать на него ссылку. **ModuleOutput** передается в **Configurator** вызываемого модуля и там устанавливается в **Presenter**. 
Содержит в себе методы модуля, которые влияют на поведение вызывающего модуля.

Пример **ModuleOutput** модуля профиля, при помощи которого можно сообщать об изменении профиля вызывающему модулю.
```swift
protocol ProfileModuleOutput: class {
    /// Notify that user profile edited
    func profileEdited()
}
```

#### Router

**Router** отвечает за конфигурацию и отображение других модулей. Под другими модулями не обязательно подразумевается *ViewController*. Это может быть дочерняя *UIView*, какое-либо всплывающее сообщение об ошибке и т.п.

##### RouterInput 

– реализуется **Router**, ссылку держит **Presenter**.
Описывает набор методов. Они нужны, чтобы отобразить другой модуль.

Пример **RouterInput** модуля профиля. С ним можно показать модуль редактирования профиля.

```swift
protocol ProfileRouterInput {
	  /// Method for transition to profile module
    func showEditProfileModule()
}
```

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

```
func showConfirmationAlert()
```

**Хороший пример:**

```
func showConfirmationModule()
```

> Мы стараемся не завязываться на детали реализации отображаемого модуля. Таким образом, мы в любой момент сможем поменять алерт на модальное окно, не трогая  протокол.  

#### Configurator

У **Configurator** нет протоколов. Он содержит только набор методов конфигурации модуля с разными входными данными.

Пример **Configurator** вышеописанного модуля профиля.

```swift
final class ProfileModuleConfigurator {

    // MARK: Internal methods

    func configure(with profile: UserProfile) -> ProfileViewController {
        let view = ProfileViewController.controller()
        let presenter = ProfilePresenter()
        let router = ProfileRouter()

        presenter.view = view
        presenter.router = router
        router.view = view
        view.output = presenter

	  presenter.configure(with: profile)

        return view
    }

}
```

## Лучшие практики
### Работа с коллекциями
#### Адаптеры
Во многих приложениях более половины экранов — коллекции *UITableView* или *UICollectionView*. Важно выбрать правильный подход разработки данных элементов, чтобы не проиграть в скорости и поддержке готовых решений.

Реализация *UITableViewDelegate* и *UITableViewDataSource* (или *UICollectionViewDelegate* и *UICollectionViewDataSource*)  **ViewController** не соответствует [принципу единственной ответственности](https://ru.wikipedia.org/wiki/Принцип_единственной_ответственности). Мы выделили отдельный объект **Adapter**, который исполняет эти протоколы. 

**Adapter** избавляет **UIViewController** от знания о внутреннем устройстве коллекции.
**Adapter** создается в **Configurator** и хранится во **View**. **View** получает необходимые данные. Далее она передает в **Adapter** ссылку на коллекцию для регистрации ячеек и информацию для запуска коллекции.
Для передачи действий из ячеек и при нажатии на них создается специальный протокол **AdapterOutput**.

Пример конфигурации хорошо знакомого нам модуля профиля, с созданием адаптера.

```swift
func configure() -> ProfileViewController {
	let view = ProfileViewController.controller()
	let presenter = ProfilePresenter()
	let router = ProfileRouter()
	let profileViewAdapter = ProfileViewAdapter(output: presenter)
	presenter.view = view
	presenter.router = router
      router.view = view
      view.output = presenter
      view.adapter = profileViewAdapter

      return view
}
```

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
let cell = tableView.dequeueReusableCell(withIdentifier: ProfileCell.nameOfClass, for: indexPath) as! ProfileCell
cell.configure(with: subscription)
return cell
```

Мы не открываем **Adapter** внутреннее устройство ячейки. Только предоставляем набор методов для ее конфигурации.

#### ReactiveDataDisplayManager

Другой способ реализации протоколов коллекций — использование библиотеки [ReactiveDataDisplayManger](https://github.com/LastSprint/ReactiveDataDisplayManager). Она предоставляет интерфейс **DDM** (Data Display Manager). С ним можно конфигурировать коллекции по элементам при помощи генераторов ячеек.
Это удобно для коллекций с разнородными ячейками. Можно последовательно передать блоки в **DDM** и не писать огромный switch-case.
Более подробно о подходе в [репозитории проекта](https://github.com/LastSprint/ReactiveDataDisplayManager).

### Взаимодействие с UIAlertController

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
    alertController(cancelAction)
    self.present(alerController, animated: true, completion: nil)
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

### Кодогенерация

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

Подход создания ничем не отличается от внедрения в уже существующий. В качестве структуры проекта используйте [Xcode-шаблон](https://github.com/surfstudio/Xcode-Project-Templates). Ниже структура проекта. 
```
Project
  Project
    Application
      AppDelegate.swift
      Info.plist
    Library
      Base Classes
        BaseClass1.swift
        BaseClass2.swift
      Extensions
        Extension1.swift
        Extension2.swift
      Protocols
        Protocol1.swift
        Protocol2.swift
      Reusable Layer
        ReuseView1.swift
        ReuseView2.swift
      Utils
        UtilsClass1.swift
        UtilsClass2.swift
    Models
      Entities
        Model1.swift
        Model2.swift
      Entry
        EntryModel1.swift
        EntryModel2.swift
    Resources
      Constants
        Closures.swift
        Constants.swift
        Colors.swift
      Fonts
        SF-UI-Text-Bold.otf
        SF-UI-Text-Heavy.otf
      Images
        Assets.xcassets
      Strings
        Localizable.strings
    User Stories
      UserStory1
        Module1
          Configurator
            Module1Configurtor.swift
          Presenter
            Module1Presenter.swift
            Module1ModuleInput.swift
            Module1ModuleOutput.swift
          Router
            Module1Router.swift
            Module1RouterInput.swift
          View
            Module1ViewController.swift
            Module1ViewInput.swift
            Module1ViewOutput.swift
            Module1ViewController.xib
        UserStory2
          ...
        Services
          Service1
            Service1.swift
            Requests
              GetSomething.request
        ThirdParty
          SomeThirdPartyFramework
            SomeDependency.swift
  ProjectTests
    Info.plist
    Helpers.swift
    Resources
    Tests
      Extensions
      Models
      UserStories
      Services
```


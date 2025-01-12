[GIT](#git)

[Команды BASH](#commandsBASH)

[Команды GIT](#commandsGIT)

[Об SSH](#aboutSSH)

[Проверка наличия SSH-ключа](#checkSSH)

[Инструкция по генерации SSH-ключа](#generationSSH)

[Инструкция по связыванию SSH-ключа и GitHub-аккаунта](#bindSSHandGitHub)

<a name="git"><h1>GIT</h1></a>

Историю проектов программистов хранит отдельная программа — **система контроля версий** (англ. _Version Control System_, или коротко _VCS_).

Для обозначения систем контроля версий используют не только аббревиатуру VCS, но и **SCM** (от англ. _Source Control Management_ — «система управления исходным кодом»).

Одно изменение или группу изменений в VCS называют **ревизией** или **версией**. Каждая такая ревизия содержит информацию о том, что изменилось, кто внёс изменения, когда это было и иногда комментарии к изменению.  
На своем ПК я использую Windows 11, поэтому далее разговор ведется с учетом данной ОС.

В повседневной работе большинство пользователей Git используют консоли с наборами команд, похожие на те, что применяют в macOS и Linux. Вы будете учиться делать то же самое. Для этого нужно установить специальный консольный инструмент для Windows, который называется **Git Bash**. Данная программа является консольной.

<a name="commandsBASH"><h2>Команды BASH</h2></a>

Для работы с помощью консольного приложения необходимо знать некоторые команды. Стоит упомянуть о том, что символ **~** - домашняя директория, **..** - на каталог выше, **.** - текущая папка. 

1. **pwd** - показывает рабочую папку.
2. **cd** - сменить директорию.
3. **ls** - отобразить содержимое директории. Флаг **-a** выводит расширенный список. В нём отобразятся все скрытые файлы, которые начинаются с символа **.** (например, файлы конфигурации). Также происходит работа с **~**.
4. **touch** - создать файл: **touch имя_файла.расширение_файла**. Например, **touch file.txt**
5. **mkdir** - создать директорию: **mkdir new_dir** - создается новая папка в текущей папке. Также можно создать целую структуру директорий одной командой с помощью флага **-p**: **mkdir -p dir1/dir-inside/dir-deeper-inside**.
6. **cp** - копирование файлов: **cp что_копируем куда_копируем**. Например, **cp d:/file.txt c:/first-project/**
7. **mv** - перемещение файлов и папок: **mv что_перемещаем куда_перемещаем**. Например, **mv d:/file01.txt c:/first-project/**
8. **cat** - чтение файлов: **cat file.txt**. Команда **cat** работает только с текстовыми файлами. Вывести этой командой файл другого типа (например, изображение) не получится.
9. **rm** - удалить файл: **rm file02.txt** - происходит удаление файла **file02.txt** в текущей папке.
10. **rmdir** - удаление папки (но пустой папки!): **rmdir c:/first-project/images** - удаляется **_пустая(!)_** папка **images** в папке **c:/first-project**.
11. **rm -r** - удаление папки и файлов, положенных в данную папку: **rm -r c:/first-project/images** - удаляется папка полностью.

<a name="commandsGIT"><h2>Команды GIT</h2></a>

1. **git version** - показывает версию Git.
2. **git config** - конфигурация Git. Вызывается так: 
- **git config --global user.name** _имя_пользователя_ -  необходимо указать своё имя или никнейм,
- **git config --global user.email** _почта_пользователя_ - указывается электронная почта.

Все глобальные настройки Git хранит в файле **.gitconfig** в домашней директории. Команда запишет в этот файл указанные имя и почту. Чтобы убедиться в этом, можно вызвать команду для чтения файлов: 

```BASH
$cat ~/.gitconfig 
```

либо 

```BASH
git config --list
```

3. **git init** - cделать папку репозиторием.
4. **rm -rf .git** - если вы случайно сделали Git-репозиторием не ту папку, её можно «разгитить».
5. **git status** - проверить состояние репозитория.
6. **git add** - подготовить файлы к сохранению. **git add --all** - позволяет подготовить к сохранению все файлы в репозитории. Также можно добавить текущую папку целиком — в этом случае все файлы в ней тоже будут добавлены. Обратиться к текущей папке в Bash позволяет точка: **git add .**.
7. **git commit** - выполнить коммит, точнее **git commit -m 'Комментарий'**.
8. **git log** - просмотреть историю коммитов.
9. **git push** - отправить изменения на удалённый репозиторий. В первый раз эту команду нужно вызвать с флагом **-u** и параметрами **origin** (имя удалённого репозитория) и **main** или **master** (название текущей ветки). Флаг **-u** свяжет локальную ветку с одноимённой удалённой. Как вы связывали локальный и удалённый репозитории, так же и здесь нужно дополнительно связать ветки: **git push -u origin main**. Если вы указывали кодовую фразу при настройке SSH-ключей, её нужно будет ввести _(читать ниже)_.
10. **git clone _адрес_** - клонировать репозиторий по https или ssh: **git clone https://github.com/yandex-praktikum/git-clone-practice.git** или **git clone git@github.com:yandex-praktikum/git-clone-practice.git**
11. **git remote -v** - проверка того, что репозитории связаны.

<a name="aboutSSH"><h2>Об SSH</h2></a>

**SSH** (англ. Secure Shell — «безопасная оболочка») — сетевой протокол прикладного уровня, позволяющий производить удалённое управление операционной системой и туннелирование TCP-соединений (например, для передачи файлов). Схож по функциональности с протоколами Telnet и rlogin, но, в отличие от них, шифрует весь трафик, включая и передаваемые пароли. SSH допускает выбор различных алгоритмов шифрования.  
Файлы настроек хранятся по пути **~/.ssh/**.

SSH использует пару ключей для обеспечения безопасности — публичный и приватный:
* **Приватный ключ** (англ. _private key_) хранится только на вашем компьютере и не должен передаваться кому-либо ещё. Он используется для расшифровки данных.
* **Публичный ключ** (англ. _public key_) доступен всем и используется для шифрования данных. Они могут быть расшифрованы парным приватным ключом.  
Только вы можете расшифровать данные приватным ключом, но любой владелец публичного ключа может их для вас зашифровать. Эти два ключа связаны и образуют **SSH-пару**.

<a name="checkSSH"><h3>Проверка наличия SSH-ключа</h3></a>

Прежде чем генерировать SSH-ключи, убедитесь, что у вас их ещё нет. По умолчанию директория с SSH-ключами находится в домашней директории. Перейдите в неё.

```BASH
$ cd ~
```

Обычно SSH-ключи находятся в директории **.ssh/**. Проверить наличие этой директории и файлов в ней можно с помощью следующей команды.

```BASH
$ ls -la .ssh/
```

Если папка пустая или её нет, всё в порядке. Если есть файлы с похожими названиями, SSH-ключи уже создавались:
* id_dsa.pub
* id_ecdsa.pub
* id_ed25519.pub
* id_rsa.pub

Если вы не создавали эти файлы, удалите их все.

<a name="checkSSH"><h3>Инструкция по генерации SSH-ключа</h3></a>

1. Для генерации SSH-пары можно использовать программу ssh-keygen. Откройте терминал и введите следующую команду:
```BASH
 $ ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"
```

Используйте электронную почту, к которой привязан ваш GitHub-аккаунт.

Если вы видите сообщение об ошибке, то, скорее всего, ваша система не поддерживает алгоритм шифрования ed25519. Ничего страшного: используйте другой алгоритм.

```BASH
 $ ssh-keygen -t rsa -b 4096 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"
```

2. Укажите место хранения ключей. Простой вариант — сделать домашний каталог пользователя путём по умолчанию. Для этого нажмите Enter.

```BASH
 > Enter a file in which to save the key (C:\Users\<имя_пользователя>\.ssh\):[Press enter]
```

Теперь в указанной директории появится пара ключей.

3. Программа запросит **кодовую фразу** (англ. _passphrase_) для доступа к SSH-ключу. Вы можете оставить поле пустым. Для этого нажмите Enter, а затем ещё раз Enter.

```BASH
 > Enter passphrase (empty for no passphrase): [Type a passphrase]
 > Enter same passphrase again: [Type passphrase again]
```

4. Готово! Теперь осталось проверить, что ключи действительно сгенерировались. Для этого вызовите следующую команду.

```BASH
 ls -a ~/.ssh
```

На экране должны появиться два файла — один с расширением .pub, другой — без. Файл в .pub — публичный, им можно делиться с веб-сайтами или коллегами. Файл без расширения .pub — приватный. Ни в коем случае не передавайте его никому!

<a name="bindSSHandGitHub"><h3>Инструкция по связыванию SSH-ключа и GitHub-аккаунта</h3></a>

1. После выполнения ssh-keygen в директории ~/.ssh есть два файла — id_ed25519 и id_ed25519.pub (или id_rsa и id_rsa.pub — в зависимости от того, какой алгоритм вы использовали). Скопируйте содержимое файла с публичным ключом в буфер обмена.

```BASH
 # скопировать содержимое ключа в буфер обмена:
 $ clip < ~/.ssh/id_rsa.pub
 # для ed25519:
 $ clip < ~/.ssh/id_ed25519.pub
 ```

  Если **clip** не сработает, выведите содержимое файла с помощью **cat ~/.ssh/id_rsa.pub** или **cat ~/.ssh/id_ed25519.pub** и скопируйте вывод в буфер обмена из консоли.

  2. Перейдите на GitHub и выберите пункт **Settings** (англ. _«настройки»_) в меню аккаунта.

  3. В меню слева нажмите на пункт **SSH and GPG keys**.

  4. В открывшейся вкладке выберите **New SSH key** (англ. _«новый SSH-ключ»_).

  5. В поле **Title** (англ. _«заголовок»_) напишите название ключа. Например, **Personal key** (англ. _«личный ключ»_).

  6. В поле **Key type** (англ. _«тип ключа»_) должно быть **Authentication Key** (англ. _«ключ аутентификации»_).

  7. В поле **Key** скопируйте ваш ключ из буфера обмена.

  8. Нажмите на кнопку **Add SSH key** (англ. _«добавить SSH-ключ»_).

  9. Проверьте правильность ключа с помощью следующей команды.

  ```BASH
  $ ssh -T git@github.com
  ```

Если это первый раз, когда вы используете Git, чтобы поделиться проектом на GitHub, появится похожее предупреждение.

```BASH
 The authenticity of host 'github.com (140.82.121.4)' can't be established. ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU. This key is not known by any other names. Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Это предупреждение сообщает, что вы никогда не соединялись с сервером GitHub. Поэтому Git не может гарантировать, что сервер является тем, за кого он себя выдаёт.  
Для подтверждения подлинности сервер генерирует и публикует ключи SHA256. Вы можете проверить ключи GitHub по этой ссылке. Если ключ в предупреждении совпадает с тем, что вы видите на сайте, значит, сервер является действительным. Введите _yes_, чтобы продолжить. Вы увидите приветствие на экране.

```BASH
 Hi %ВАШ_АККАУНТ%! You've successfully authenticated, but GitHub does not provide shell access.
```
# CICD

## Подготовка к выполнению

1. Создайте два VM в Yandex Cloud с параметрами: 2CPU 4RAM Centos7 (остальное по минимальным требованиям).

![BM in YandexCloud](https://user-images.githubusercontent.com/109212419/225990785-0b0cba2b-2e35-4c05-9aa8-b629484d0195.jpg)

2. Пропишите в [inventory](./infrastructure/inventory/cicd/hosts.yml) [playbook](./infrastructure/site.yml) созданные хосты.
3. Добавьте в [files](./infrastructure/files/) файл со своим публичным ключом (id_rsa.pub). Если ключ называется иначе — найдите таску в плейбуке, которая использует id_rsa.pub имя, и исправьте на своё.
4. Запустите playbook, ожидайте успешного завершения.

![Deployment via playbook](https://user-images.githubusercontent.com/109212419/225990868-73b72d6d-3a88-4b3e-87a0-b2078c296884.jpg)

5. Проверьте готовность SonarQube через [браузер](http://localhost:9000).
6. Зайдите под admin\admin, поменяйте пароль на свой.

![Sonarcube работает](https://user-images.githubusercontent.com/109212419/225991241-78ff05fb-8e8d-4a58-ac45-8dbff26a507f.jpg)

7. Проверьте готовность Nexus через [бразуер](http://localhost:8081).
8. Подключитесь под admin\admin123, поменяйте пароль, сохраните анонимный доступ.

![Nexus вошли под админом](https://user-images.githubusercontent.com/109212419/225991304-6b8371b2-626a-4ada-b289-961dcd329f0d.jpg)

## Знакомоство с SonarQube

### Основная часть

1. Создайте новый проект, название произвольное.
2. Скачайте пакет sonar-scanner, который вам предлагает скачать SonarQube.
3. Сделайте так, чтобы binary был доступен через вызов в shell (или поменяйте переменную PATH, или любой другой, удобный вам способ).
4. Проверьте `sonar-scanner --version`.

![Sonar готов к работе](https://user-images.githubusercontent.com/109212419/225991582-f882f912-df7d-4365-9d6b-5935ff395e90.jpg)

5. Запустите анализатор против кода из директории [example](./example) с дополнительным ключом `-Dsonar.coverage.exclusions=fail.py`.

![прогон sonara](https://user-images.githubusercontent.com/109212419/225991636-a180f7c5-b681-4ba1-bd1e-7942d7fb748d.jpg)

6. Посмотрите результат в интерфейсе.

![вид на ВМ](https://user-images.githubusercontent.com/109212419/225991658-8344ad22-eb08-4fd5-bb13-77e0e05b110a.jpg)

7. Исправьте ошибки, которые он выявил, включая warnings.

![сонар прогон после исправления ошибок](https://user-images.githubusercontent.com/109212419/225991740-1af3251e-7841-47c5-be71-a2231fa3585b.jpg)

8. Запустите анализатор повторно — проверьте, что QG пройдены успешно.
9. Сделайте скриншот успешного прохождения анализа, приложите к решению ДЗ.

![вид на ВМ после исправления ошибок](https://user-images.githubusercontent.com/109212419/225991793-d1b86356-dccf-4d83-a0a6-cff2ffd926dc.jpg)

## Знакомство с Nexus

### Основная часть

1. В репозиторий `maven-public` загрузите артефакт с GAV-параметрами:

 *    groupId: netology;
 *    artifactId: java;
 *    version: 8_282;
 *    classifier: distrib;
 *    type: tar.gz.
   
2. В него же загрузите такой же артефакт, но с version: 8_102.
3. Проверьте, что все файлы загрузились успешно.

![Nexus артефакты загружуны](https://user-images.githubusercontent.com/109212419/225992744-cfd28ad1-4fae-47c1-8608-ccff1df55a2d.jpg)

4. В ответе пришлите файл `maven-metadata.xml` для этого артефекта.
![maven-metadata.xml](https://github.com/ALEMOLOKOV/9.3_CICD_Aleksandr_Molokov/blob/main/maven-metadata.xml)

### Знакомство с Maven

### Подготовка к выполнению

1. Скачайте дистрибутив с [maven](https://maven.apache.org/download.cgi).
2. Разархивируйте, сделайте так, чтобы binary был доступен через вызов в shell (или поменяйте переменную PATH, или любой другой, удобный вам способ).
3. Удалите из `apache-maven-<version>/conf/settings.xml` упоминание о правиле, отвергающем HTTP- соединение — раздел mirrors —> id: my-repository-http-unblocker.
4. Проверьте `mvn --version`.

![Maven настроен](https://user-images.githubusercontent.com/109212419/225992946-b7d8401e-c548-4a53-901c-6ce25b2888b5.jpg)

5. Заберите директорию [mvn](./mvn) с pom.

### Основная часть

1. Поменяйте в `pom.xml` блок с зависимостями под ваш артефакт из первого пункта задания для Nexus (java с версией 8_282).
2. Запустите команду `mvn package` в директории с `pom.xml`, ожидайте успешного окончания.
3. Проверьте директорию `~/.m2/repository/`, найдите ваш артефакт.

![Maven успешная сборка и проверка](https://user-images.githubusercontent.com/109212419/225993069-60290524-8759-44ae-9f81-c52d246837e4.jpg)

4. В ответе пришлите исправленный файл `pom.xml`.

![pom.xml](https://github.com/ALEMOLOKOV/9.3_CICD_Aleksandr_Molokov/blob/main/pom.xml)

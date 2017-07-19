# Описание unity-webview

Unity-webview - это плагин для Unity 5-2017, который накладывает компоненты WebView
на представление Unity рендера. Он работает на Android, iOS, Unity Web Player и OS X
(Windows пока не поддерживается).

unity-webview получен от keijiro-san's
https://github.com/keijiro/unity-webview-integration

## Пример проекта

Готовый пример размещен в директории `sample/`. Вы прото можете открыть эту директорию и импортировать как описсано ниже:

1. Открыть `sample/Assets/Sample.unity`.
2. Открыть `dist/unity-webview.unitypackage` и импортировать Unity ассет (не содержит примеров). Или можете распаковать архив `dist/unity-webview.zip` и заменить файлы проекта, если например unity-webview был импортирован раньше...

## Примечания по платформам 

### OS X (Редактор)

#### При переходе на Unity 2017.1

Плагин не компилируется в Unity 2017.1. Для решения проблемы выполните следующие действия:

```bash
$ cd /Applications/Unity/Unity.app/Contents/Frameworks/
$ ln -s Mono/MonoEmbedRuntime .
```

Взято отсюда: https://github.com/gree/unity-webview/issues/219#issuecomment-315760749

#### Обеспечение безопасности приложения

Начиная с Unity 5.3.0, Unity.app построен с ATS (App Transport Security) и незащищенные соединения по протоколу HTTPзапрещены. Если вы всётаки хотите открыть `http:` соединение с помощью этого плагина Вам нужно внести изменения в Unity OS X Editor.
Для этого Вам нужно открыть в текстовом редакторе файл:
`/Applications/Unity5.3.4p3/Unity.app/Contents/Info.plist` 
и добавить следующеий блок:

```diff
--- Info.plist~	2016-04-11 18:29:25.000000000 +0900
+++ Info.plist	2016-04-15 16:17:28.000000000 +0900
@@ -57,5 +57,10 @@
 	<string>EditorApplicationPrincipalClass</string>
 	<key>UnityBuildNumber</key>
 	<string>b902ad490cea</string>
+	<key>NSAppTransportSecurity</key>
+	<dict>
+		<key>NSAllowsArbitraryLoads</key>
+		<true/>
+	</dict>
 </dict>
 </plist>
```

Затем в терминале вызвать следующие команды:

```bash
/usr/libexec/PlistBuddy -c "Add NSAppTransportSecurity:NSAllowsArbitraryLoads bool true" /Applications/Unity/Unity.app/Contents/Info.plist
```

##### Рекомендации

* https://github.com/gree/unity-webview/issues/64
* https://onevcat.zendesk.com/hc/en-us/articles/215527307-I-cannot-open-the-web-page-in-Unity-Editor-

#### WebViewSeparated.bundle

Это вариант WebView.bundle. Он основан на https://github.com/gree/unity-webview/pull/161. Как отмечалось в
Pull-request, он отображает отдельное окно и позволяет разработчику Используйте отладчик Safari. Чтобы включить его, пожалуйста, определите константу `WEBVIEW_SEPARATED`.

### iOS

Текущая реализация плагина поддерживает WKWebView (подробнее про WKWebView читайте: https://developer.apple.com/documentation/webkit/wkwebview), но по умолчанию поддержка отключена. Чтобы включить, установите для параметра enableWKWebView следующее:

```csharp
		webViewObject.Init(
            ...
            enableWKWebView: true);
```


### Android

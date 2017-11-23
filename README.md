# Introdução ao NativeScript

# O que é **NativeScript**?

# O que não é **NativeScript**?

# Exemplo Android

```
let time = new android.text.format.Time()
time.set(1, 0, 2015)
console.log(time.format("%D"))
```
O Resultado será: 
> "01/01/2015"

<img src="https://media.giphy.com/media/7YdfM2p9FqmfC/giphy.gif">

# Exemplo IOS
```
let alert = new UIAlertView()
alert.message = "Hello World!"
alert.addButtonWithTitle("Ok")
alert.show()
```
E o resultado é: 

<img src="http://i.imgur.com/3Z9nB19.png">

# Mas como é que isso funciona?

**NativeScript e JS VMs**

- NativeScript executa JavaScript com o JavaScript VM
  - JavaScriptCore no IOS
  - V8 no Android 
  


Sendo executado com **V8 **
```
let time = new android.text.format.Time()
time.set(1, 0, 2015)
console.log(time.format("%D"))
```

Sendo executado com **JavascriptCore**
```
let alert = new UIAlertView()
alert.message = "Hello World!"
alert.addButtonWithTitle("Ok")
alert.show()
```
# Utilizando as API's Nativas
- NativeScript usa reflexão para construir uma lista de APIs disponíveis para cada plataforma.
- Para um ótimo desempenho, este metadata é gerado previamente, e injetado no pacote do aplicativo em tempo de construção
  
# Injetando API Nativa
- V8/JavascriptCore tem API's para injetar nas variaveis globais

<img src="https://img.scoop.it/cClViuEs1z2eATZzxdO_JDl72eJkfbmt4t8yenImKBVvK0kTmF0xjctABnaLJIm9">

# Invocando API's
`let time = new android.text.format.Time()`

- V8 / JavaScriptCore tem retorno de chamada C++ para a função JS chamadas e acessos de propriedade.  
- O tempo de execução do NativeScript usa esses callbacks para traduzir as chamadas JS para chamadas nativas.
- No iOS, você pode chamar diretamente as APIs Objective-C do C++.
- No Android, o NativeScript usa o JNI do Android (Java Interface Nativa) para fazer a ponte de C ++ para Java.
`let time = new android.text.format.Time()`
- (1) O retorno de chamada da função V8 é executado.
- (2) O tempo de execução do NativeScript usa seus metadados para saber que `Time()` significa que precisa instanciar um Objeto `android.text.format.Time`.
- (3) O tempo de execução do NativeScript usa o JNI para instanciar um objeto `android.text.format.Time` e mantém uma referência a ele.
- (4) O tempo de execução do NativeScript retorna um objeto JS que proxy o objeto JavaTime.
- (5) Controle retorna para JS onde o objeto proxy é armazenado como uma variável de tempo local.
```
let time = new android.text.format.Time()
time.set(1, 0, 2015)
console.log(time.format("%D"))
```

<img src="https://media1.tenor.com/images/e1cfabd510b87fb46c5879ed521f6dc0/tenor.gif?itemid=5875361">

> Então você escreve apenas codigo nativo? **NÃO**

# TNS Modules
- Módulos fornecidos com NativeScript que oferecem funcionalidade crossplatform.
- Há dezenas deles e são fáceis de escrever você mesmo.
- Os módulos TNS seguem as convenções do node module (CommonJS).

# TNS File Module
```
const fileSystemModule = require("file-system")
new fileSystemModule.File(path)
```
> Basicamente isso, se tornara os trechos a seguir...


```
// ANDROID
new java.io.File(path)

// IOS
NSFileManager.defaultManager()
fileManager.createFileAtPathContentsAttributes(path)

```

# Modulo HTTP
```
const http = require('http')
http.getJSON("https://api.meuservico.com")
.then(response => {
  // Resultado
})
```


# Custom Modulos TNS
```
// device.ios.js
module.exports = {
  version: UIDevice.currentDevice().systemVersion
}

// device.android.js
module.exports = {
  version: android.os.Build.VERSION.RELEASE
}
```

# Usando o Custom Modulo
```
const device = require('./device')
console.log(device.version)
```

# Como inicio meu projeto?
- (1) Instalar NativeScript globalmente
  - `npm install -g nativescript`
- (2) Requesitos NativeScript CLI
  - JDK, Apache Ant, Android SDK
  - Xcode, Xcode CLI Tools, iOS SDK
  
# Iniciando um novo Projeto

Depois que já instalou o `NativeScript`
`tns create hello-world && cd hello-world`

# Executando no iOS
`tns platform add ios && tns run ios --emulator`

<img src="http://developer.telerik.com/wp-content/uploads/2015/09/TNSiOS.jpg">

# Executando no Android
`tns platform add android && tns run android --emulator`

<img src="http://developer.telerik.com/wp-content/uploads/2015/09/TNSAndroid.jpg">

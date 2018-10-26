# React Camera Studing

## [Manipulando a câmera no React Native com o react-native-camera](https://blog.rocketseat.com.br/react-native-camera/)

#### Utilizando a biblioteca react-native-camera para tirar fotos no React Native

### Iniciando o projeto

Para iniciar o projeto vamos utilizar o React Native CLI, com ele instalado basta executar o comando:

```
react-native init ReactNativeCamera
```

### Com o projeto criado vamos instalar o [react-native-camera](https://github.com/react-native-community/react-native-camera):

```
yarn add react-native-camera
// npm install react-native-camera
```

E como se trata de um recurso nativo do dispositivo temos que criar o link entre o Javascript e o Código Nativo, e para isso basta executar:

```
react-native link react-native-camera
```

### Configurando o react-native-camera

#### Android

Já se você tentar abrir o aplicativo no Android Studio, provavelmente irá acontecer o erro mostrado abaixo.

```
Gradle 'android' project refresh failed
    Could not find method google() for arguments [] on repository container.
    
```

E isso acontece devido a versão do Gradle instalada no projeto, por padrão o React Native CLI cria o projeto com a versão 2.2.3 instalada, e para que o react-native-camera funcione corretamente teremos que fazer a atualização para a versão 3.1.0, o processo é um pouco trabalhoso, mas não se preocupe que te mostrarei em detalhes como realizá-lo.

Primeiramente vamos modificar a URL de onde são buscadas as informações do Gradle, para isso vamos modificar o arquivo ```android/gradle/wrapper/gradle-wrapper.properties```, ficando assim:

```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-4.4-all.zip

```

Feito isso o próximo passo é atualizar o arquivo ```android/build.gradle```, ficando assim:

```

buildscript {
  repositories {
    ...
    google() // Linha Adicionada
  }
  dependencies {
    // Versão modificada de 2.2.3 para 3.1.0
    classpath 'com.android.tools.build:gradle:3.1.0'
  }
}
allprojects {
  repositories {
    ...
    google() // Linha Adicionada
    maven { url "https://jitpack.io" } // Linha Adicionada
    maven {
      url "$rootDir/../node_modules/react-native/android"
    }
  }
}
// Adicionar todo o trecho abaixo
subprojects {
  project.configurations.all {
    resolutionStrategy.eachDependency { details ->
      if (details.requested.group == 'com.android.support'
          && !details.requested.name.contains('multidex') ) {
        details.useVersion "26.1.0"
      }
    }
  }
}

```

E por último temos que modificar a versão do SDK e do Build Tools usados para o build do aplicativo no arquivo ```android/app/build.gradle```, no seu projeto eles devem estar com a versão 23, que vem por padrão, basta mudar ambos para a versão 26, como abaixo:


```

...
android {
    compileSdkVersion 26
    buildToolsVersion "26.0.1"
...

```

Agora salve todos seus arquivos, cruze os dedos e abra o projeto novamente no Android Studio, dessa vez ele deve conseguir completar o processo com sucesso, mesmo com alguns Warnings, os quais você não precisa se preocupar.

Para testar abra o AVD do Android Studio ou outra alternativa de emulador e execute:

```

react-native run-android

```




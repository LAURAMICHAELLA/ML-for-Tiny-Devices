# ML for Tiny Devices (based on sbai-course) - ESP32

This is based on [SBAI 2023](https://sbai2023.com.br/sbai/) minicourse material. The course title is ***Portando Modelos IA para microcontroladores de 32 bits***.

Follow steps below to create course environemnt and execute the lessons.

# Python Environment Setup

1. First, install ´pip´ and ´virtualenv´:
```bash
sudo apt-get install python3-pip -y
pip3 install virtualenv 
```

2. Create and activate virtualenv for your user, example (`.sbai`):
```bash
python3 -m virtualenv .sbai
source .sbai/bin/activate
```

3. Install remain python dependecies:
```bash
pip install -r requirements.txt
```

# Arduino Environment Setup

1. Download and unzip [Arduino IDE - `Linux ZIP file 64 bits (X86-64) version`](https://www.arduino.cc/en/software);

2. Make `arduino-ide` executable and install it:
```
chmod +x arduino-ide
./arduino-ide
```

3. Open Arduino IDE. Go to `File > Preferences`;

4. Enter the following link into the `Additional Board Manager URLs` field and click **OK**:
```
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json 
```

5. Open the Boards Manager. Go to `Tools > Board >Boards Manager` and search for ESP32.

6. Install the ESP32 by Espressif Systems;

7. If you have some USB permission issue like:

>```cpp
>fatal error occurred: Could not open /dev/ttyACM0, the port doesn't exist
>Failed uploading: uploading error: exit status 2
>```

**Type:**
```bash
sudo adduser $USER $(stat --format="%G" /dev/ttyACM0 )
```

8. Reboot your PC and try again.


# microML para Arduino: Classificação do Dataset Iris
Este projeto demonstra como implementar um classificador SVM para o dataset Iris usando a biblioteca microML no Arduino.

microML é uma implementação simplificada de machine learning para microcontroladores como Arduino, permitindo que você execute modelos de aprendizado de máquina diretamente em dispositivos embarcados.

# Como utilizar microML no Arduino
Pré-requisitos:
Instale a biblioteca microML no Arduino IDE:

Vá em "Sketch" > "Incluir Biblioteca" > "Gerenciar Bibliotecas"

# Busque por microml e instale a biblioteca

Prepare seu modelo de machine learning (geralmente criado em Python com scikit-learn) e converta-o para formato compatível com Arduino.
Passos básicos:
Treine seu modelo em Python
Use o gerador de código do microML para exportar o modelo
Copie o código gerado para seu sketch Arduino
Implemente a lógica de coleta de dados e predição

# ⚠️ Atenção
O Arduino não carrega o dataset completo devido a limitações de memória.
O modelo é pré-treinado em Python e exportado para C++.
Novos dados são classificados em tempo real (via sensores ou arrays fixos).

# Código completo
#include <microml.h>

// 1. MODELO PRÉ-TREINADO (exportado do Python)
const float supportVectors[] = { /* ... */ };  // Vetores do SVM
const float coefficients[] = { /* ... */ };    // Coeficientes do modelo
const float intercept = -0.0588;               // Viés (bias)
SVMClassifier classifier(/* ... */);           // Classificador

void setup() {
    Serial.begin(9600);
}

void loop() {
    // 2. DADOS DE ENTRADA (substitua isso pelos seus dados reais)
    float flowerData[4] = {
        5.1,  // Comprimento da sépala (cm)
        3.5,   // Largura da sépala (cm)
        1.4,   // Comprimento da pétala (cm)
        0.2    // Largura da pétala (cm)
    };

    // 3. CLASSIFICAÇÃO (o modelo usa os dados acima)
    int classId = classifier.predict(flowerData);

    // Exemplo de saída:
    Serial.print("Classe: ");
    Serial.println(classId);  // 0=Setosa, 1=Versicolor, 2=Virginica
    delay(1000);
}
📋 Explicação do Código Arduino
1. Modelo Pré-treinado (C++)
// Modelo SVM exportado do Python
const float supportVectors[] = { /* ... */ };  // Vetores de suporte
const float coefficients[] = { /* ... */ };    // Coeficientes do modelo
const float intercept = -0.0588;               // Viés (bias)
SVMClassifier classifier(/* ... */);           // Instância do classificador
2. Dados de Entrada
// Dados simulados (substituir por leituras de sensores)
float flowerData[4] = {
    5.1,  // Comprimento sépala (cm)
    3.5,  // Largura sépala (cm)
    1.4,  // Comprimento pétala (cm)
    0.2   // Largura pétala (cm)
};
3. Classificação
// 3. CLASSIFICAÇÃO (o modelo usa os dados acima)
    int classId = classifier.predict(flowerData);

    // Exemplo de saída:
    Serial.print("Classe: ");
    Serial.println(classId);  // 0=Setosa, 1=Versicolor, 2=Virginica
4. 🧠 Alteração no código para teste com amostras Dataset Iris (Opcional)
Para testes com amostras limitadas (3 exemplos):

// Dataset Iris reduzido (exemplo com 3 amostras)
const float irisDataset[3][4] = {
    {5.1, 3.5, 1.4, 0.2},  // Setosa
    {7.0, 3.2, 4.7, 1.4},   // Versicolor
    {6.3, 3.3, 6.0, 2.5}    // Virginica
};
const int irisLabels[3] = {0, 1, 2};  // Labels correspondentes

void loop() {
    // Classifica cada amostra
    for (int i = 0; i < 3; i++) {
        int prediction = classifier.predict(irisDataset[i]);
        Serial.print("Amostra ");
        Serial.print(i);
        Serial.print(": ");
        Serial.println(prediction == irisLabels[i] ? "Correto" : "Erro");
    }
    delay(5000);
}
Observação: provavelmente você não conseguirá carregar o dataset completo devido a limitação de memória: O Arduino Uno (por exemplo) tem apenas 2KB de RAM. O dataset Iris completo (150 amostras × 4 features) não caberia.

5. 🔄 Fluxo de Trabalho
Treinar a base em python utilizando qualquer IDE que compile o arquivo .py:
from sklearn.svm import SVC
from micromlgen import port
from sklearn.datasets import load_iris

# Carrega o dataset Iris
X, y = load_iris(return_X_y=True)

# Treina o modelo
model = SVC(kernel='linear').fit(X, y)

# Gera o código C++ para o Arduino
print(port(model))  # Copie a saída para substituir no código Arduino
Observação:
O dataset é carregado apenas durante o treinamento. O Arduino só recebe o modelo já treinado (não o dataset completo).

6. Implementar no Arduino
#include <microml.h>
#include <Arduino.h>

// 1. Modelo SVM pré-treinado (exportado do Python)
const float supportVectors[] = { /* ... */ };  // Vetores de suporte
const float coefficients[] = { /* ... */ };    // Coeficientes
const float intercept = -0.0588;               // Bias
const int nClasses = 3;
const int nSupports = 3;
const int nFeatures = 4;

SVMClassifier classifier(
    supportVectors,
    coefficients,
    intercept,
    nClasses,
    nSupports,
    nFeatures
);

void setup() {
    Serial.begin(9600);
    while (!Serial);
    Serial.println("Classificador Iris com microML");
}

void loop() {
    // 2. Dados de entrada (simulados ou de sensores)
    float input[4] = {5.1, 3.5, 1.4, 0.2};  // Exemplo: Iris Setosa
    
    // 3. Classificação
    int prediction = classifier.predict(input);
    
    // 4. Saída
    Serial.print("Classe prevista: ");
    switch(prediction) {
        case 0: Serial.println("Iris Setosa"); break;
        case 1: Serial.println("Iris Versicolor"); break;
        case 2: Serial.println("Iris Virginica"); break;
    }
    delay(2000);
}
6. O que você precisa fazer:
Substituir os placeholders (/* ... */) pelos valores reais do seu modelo exportado do Python (usando micromlgen).

Adaptar os dados de entrada:

Pode ser um array fixo (como no exemplo) ou Dados lidos de sensores (ex: float input[4] = {sensor1.read(), sensor2.read(), ...};).

⚠️ Limitações Técnicas
Componente	Arduino Uno/Nano	Arduino + SD Card	ESP32/ESP8266
Memória RAM	2KB (~20 amostras)	1-2KB livres após SD	320KB+ (livre)
Armazenamento	32KB (Flash)	Até 1MB (arquivo .txt)	4MB+ (SPIFFS/LittleFS)
Velocidade de Leitura	N/A (dados embutidos)	~10-50ms/linha	~5-20ms/linha
Formato Suportado	Arrays no código	CSV simples	JSON/CSV
Custo	$	$$	$$$
Complexidade	Modelos Lineares	$$	Redes
🚀 Complexidade Computacional vs Hardware
Algoritmo	Arduino Uno/Nano (ATmega328P)	Arduino + SD Card	ESP32/ESP8266
SVM Linear	🟡 (Até 3 features)	🟢 (Até 10 features)	🟢 (Até 100 features)
Árvore	🟢 (Profundidade ≤ 5)	🟢 (Profundidade ≤ 10)	🟢 (Profundidade ≤ 20)
KNN	🔴 (Inviável)	🟡 (Até 15 amostras)	🟢 (Até 1000 amostras*)
RNA Tiny	🔴 (Inviável)	🔴 (Inviável)	🟡 (Até 3 camadas)
Critérios de Avaliação:
🟢 Viável: Execução em < 50ms, RAM < 80% livre
🟡 Limitado: Requer otimizações (ex: quantização)
🔴 Inviável: Estoura memória ou > 500ms/inferência
Chave Técnica:
Símbolo	CPU Clock	RAM Livre	Flash	Observações
Uno	16MHz	2KB	32KB	Sem acelerador matemático
+SD	16MHz	1-2KB	32KB	Overhead de leitura do SD
ESP32	160-240MHz	320KB	4MB	Acelerador de ponto flutuante
Memória: Uso típico para dataset Iris (4 features)

KNN: Armazenamento do dataset na RAM (inviável acima de 20 amostras)
*Com armazenamento em SPIFFS/LittleFS. Dados assumem 4 features por amostra.

Quando evitar:
❌ KNN no Uno (consumo RAM exponencial)
❌ RNAs não quantizadas (exceto no ESP32 com TensorFlow Lite)
⚠️ Recomendações Críticas
Evite KNN em dispositivos com < 8KB RAM.

Prefira árvores para sistemas com:

Restrição de energia

Necessidade de inferência ultra-rápida (< 5ms)

RNAs só são viáveis em ESP32 com acelerador tensor-lite.

Nota: Valores assumem dataset Iris (150 amostras × 4 features × 4 bytes = ~2.4KB).
Para projetos reais, prefira ESP32 ou enviar dados por serial/HTTP.

🔗 Recursos Úteis
Concepts
Biblioteca microML
Dataset Iris

# References

## TinyML concepts
- [An Introduction to TinyML](https://towardsdatascience.com/an-introduction-to-tinyml-4617f314aa79)
- [TinyML Foundation](https://www.tinyml.org/)
- [Unifei Material - TinyML](https://github.com/Mjrovai/UNIFEI-IESTI01-TinyML-2022.1)

## Some Tools and Tutorials
- [Micromlgen](https://eloquentarduino.com/libraries/micromlgen/)
- [Github micromlgen](https://github.com/eloquentarduino/micromlgen)
- [How to get started fast with
Arduino Machine Learning](https://eloquentarduino.com/arduino-machine-learning/)
- [Github tflite-micro](https://github.com/tensorflow/tflite-micro/tree/main)
- [First steps with ESP32 and TensorFlow Lite for Microcontrollers](https://medium.com/@dmytro.korablyov/first-steps-with-esp32-and-tensorflow-lite-for-microcontrollers-c2d8e238accf)


## Iris Dataset Description and Tutorials
- [The Iris Dataset](https://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html)
- [Iris Dataset Analysis using Python](https://www.hackersrealm.net/post/iris-dataset-analysis-using-python)
- [Iris Dataset Classification with Python: A Tutorial](https://www.pycodemates.com/2022/05/iris-dataset-classification-with-python.html)
- [Iris Data Prediction using Decision Tree Algorithm](https://medium.com/analytics-vidhya/iris-data-prediction-using-decision-tree-algorithm-7948fb68201b)

# Others
- [How to install virtualenv](https://gist.github.com/frfahim/73c0fad6350332cef7a653bcd762f08d);

# ML-for-Tiny Devices -Profa. Dra. Laura Michaella

Refere-se ao campo de **TinyML (Tiny Machine Learning)**, uma área da tecnologia que permite rodar modelos de inteligência artificial diretamente em hardwares extremamente limitados, como microcontroladores, sensores e dispositivos de Internet das Coisas (IoT).

Em vez de enviar os dados para a nuvem para serem processados, o próprio dispositivo toma as decisões localmente.

## Características Principais

* **Baixíssimo consumo:** Dispositivos operam com miliwatts (mW) ou microwatts (μW), permitindo que funcionem por meses ou anos com uma única bateria de relógio.
* **Hardware limitado:** Modelos rodam em chips com menos de 1 MB de memória RAM e armazenamento Flash.
* **Latência zero:** Como o processamento é local, a resposta é imediata, sem depender de internet.
* **Privacidade:** Os dados (como áudio ou imagens de sensores) não saem do dispositivo.

## Como um modelo cabe em um "Tiny Device"?

Modelos tradicionais de IA são gigantescos. Para fazê-los caber em microcontroladores, são usadas técnicas de otimização:

1. **Quantização:** Converte os números do modelo de alta precisão (Float32) para formatos menores (Int8), reduzindo o tamanho do arquivo em até 4 vezes.
2. **Poda (Pruning):** Remove conexões neurais que não impactam o resultado final.
3. **Destilação de Conhecimento:** Treina um modelo minúsculo para imitar o comportamento de um modelo grande.

## Hardwares e Ferramentas Comuns

* **Placas populares:** Arduino Nano 33 BLE Sense, Raspberry Pi Pico, ESP32 e chips STM32.
* **Frameworks de Software:** TensorFlow Lite for Microcontrollers (TFLite Micro), Edge Impulse e MicroTVM.

## Vamos começar?

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

 After the installations above, follow the slides directory for beginning developement in the root directory:
[SNCT 2025](SNCT2025_laura.pdf)
[SBAI 2023](SBAI 2023.pptx.pdf) or

# microML para Arduino: Classificação do Dataset Iris
Este projeto demonstra como implementar um classificador SVM para o dataset Iris usando a biblioteca microML no Arduino.

microML é uma implementação simplificada de machine learning para microcontroladores como Arduino, permitindo que você execute modelos de aprendizado de máquina diretamente em dispositivos embarcados.

Seguir o outro README: [ML_Arduino](ML_Arduino.md)

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

# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
## ПЕРЦЕПТРОН
Отчет по лабораторной работе #4 выполнил(а):
- Довгий Вадим Игоревич
- РИ-210942

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Познакомиться с перцептроном
## Постановка задачи.
В данной лабораторной работе мы в проекте Unity рализуем перцептрон, который умеет производить вычисления: OR, AND, NAND, XOR. Так же построим графики зависимости количества эпох от ошибки обучения. 
В ходе данной лабораторной работы мы в проекте Unity реализуем перцептрон, который произведёт вычисления: OR, AND, NAND, XOR. Построим графики зависимости количества эпох от ошибки обучения. Также построим визуальную модель перцептрона на сцене.

## Задание 1
### В проекте Unity рализовать перцептрон, который умеет производить вычисления: OR, AND, NAND, XOR

Для начала я создал проект. На сцене создал объект Perceptron, добавил на него скрипт, который я привёл ниже, и в Inspector изменил значения переменных скрипта:
![2022-11-28_15-42-59](https://user-images.githubusercontent.com/45125347/204310495-011297cd-8652-4122-883f-c9f5af32355e.png)

Вот сам скрипт для обучения перцептрона:
```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(15);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));		
	}
	
	void Update () {
		
	}
}
```
Результат вычисления для OR:
Перцептрон успешно обучился
![2022-11-28_16-16-05](https://user-images.githubusercontent.com/45125347/204310594-461c76b3-64b8-4c2e-ab2e-88c028584a7a.png)

Результат вычисления для AND:
Перцептрон успешно обучился
![2022-11-28_16-16-29](https://user-images.githubusercontent.com/45125347/204310678-a01eb177-fddf-4272-8425-734dc7e47451.png)

Результат вычисления для NAND:
Перцептрон успешно обучился
![2022-11-28_16-17-03](https://user-images.githubusercontent.com/45125347/204310778-64472479-4d3d-4365-8e55-cf7ad9299b69.png)

Результат вычисления для XOR:
Перцептрон не обучился даже при 9999 эпохах обучения
![2022-11-28_16-18-14](https://user-images.githubusercontent.com/45125347/204311032-ec207f28-240d-4495-a39e-c853a9bb9629.png)
![2022-11-28_16-24-52](https://user-images.githubusercontent.com/45125347/204311058-73ba0fc7-6d34-49be-a89f-da12c10ac872.png)


## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения.

Чем большее количество эпох обучения, тем меньше вероятность того, что будет ошибка, поскольку увеличивается количество попыток для обучения.
Ниже я представляю графики зависимостей количества эпох от ошибки обучения.

Для OR:

![2022-11-28_17-05-03](https://user-images.githubusercontent.com/45125347/204311849-a7becde3-a7fa-4cb4-b7ad-95db1f472b90.png)

Для AND:

![2022-11-28_17-05-37](https://user-images.githubusercontent.com/45125347/204312005-b2e47132-cfb8-4b64-9ca6-bc80bf37edc8.png)

Для NAND:

![2022-11-28_17-06-53](https://user-images.githubusercontent.com/45125347/204312092-158647c5-f764-4a35-bd38-902fbf858b6b.png)

Для XOR:
График XOR отличается от остальных графиков по причине того, что перцептрон не смог обучиться.

![2022-11-28_17-07-44](https://user-images.githubusercontent.com/45125347/204312388-127e4654-9994-4173-9fbe-4405b51a6394.png)

## Задание 3
### Построить визуальную модель работу перцептрона на сцене Unity

Для начала я создал новый скрипт, который прикрепил к кубикам. Он отвечает за смену цвета объекта при касании другого.
0 - синий цвет
1 - красный цвет

Сам скрипт:

```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ChangeColor : MonoBehaviour
{
    public Color color;

    private void OnTriggerEnter(Collider other)
    {
        other.gameObject.GetComponent<Renderer>().material.color = this.color;
    }
}
```
Все визуальные модели и к ним пояснения я приведу ниже на видео.

Модель для OR:

https://user-images.githubusercontent.com/45125347/204313727-de31ecaa-842f-4cf9-99a8-86a5a29578db.mp4

Модель для AND:

https://user-images.githubusercontent.com/45125347/204313805-d507b5f1-2bbd-492f-b112-284e1c00b962.mp4

Модель для NAND:

https://user-images.githubusercontent.com/45125347/204313885-546814f4-8fcf-47f4-8b7c-03f4575d8d5b.mp4

Модель для XOR:

https://user-images.githubusercontent.com/45125347/204313947-2d932f55-7337-4e3b-9d37-90476d88f17e.mp4

## Выводы
В данной лабораторной работе мы научились реализовывать обучение перцептрона для различных логических операций, рассмотрели графики зависимостей и построили визуальную модель перцептрона на сцене Unity

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
В проекте Unity рализовать перцептрон, который умеет производить вычисления: OR, AND, NAND, XOR

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
Построить графики зависимости количества эпох от ошибки обучения.
Чем больше количество эпох обучения тем меньше вероятность ошибки.Так как увеличивается количество попыток на обучение. 
График зависимости  количества эпох от ошибки обучения для OR:

![image](https://user-images.githubusercontent.com/114181560/204036513-521730eb-2f85-4299-9f42-5aa112a66c9f.png)

График зависимости  количества эпох от ошибки обучения для AND:
![image](https://user-images.githubusercontent.com/114181560/204036605-ef3e2669-a1c9-4f97-99ff-b866ad37bf81.png)


График зависимости  количества эпох от ошибки обучения для NAND:
![image](https://user-images.githubusercontent.com/114181560/204036623-e014577d-8dab-4ecd-a686-c5bed157fc79.png)


График зависимости  количества эпох от ошибки обучения для XOR:
Так как перцептрон не смог обучится, то и график для XOR отличается от других операций

![image](https://user-images.githubusercontent.com/114181560/204036636-f0027afe-6760-4eb5-99dd-81d81f59e7e9.png)

## Выводы
В данной лабораторной работе мы научились реализовывать обучение перцептрона для разных логических операций.

# Python 中常微分方程的龙格-库塔数值积分

> 原文：<https://medium.com/geekculture/runge-kutta-numerical-integration-of-ordinary-differential-equations-in-python-9c8ab7fb279c?source=collection_archive---------2----------------------->

## 用 Python 中的数值积分求解常微分方程

## 介绍

本文演示了如何在 Python 中使用 **Runge-Kutta (RK)** 方法数值求解 *常微分方程(ODEs)* 。

**建模** *物理现象*，如[大气对流](https://en.wikipedia.org/wiki/Lorenz_system#Model_for_atmospheric_convection)或 [*牛顿冷却定律*](https://youtu.be/IICR-w1jYcA) 所描述的物体温度变化，都包含了 ODEs。此外……
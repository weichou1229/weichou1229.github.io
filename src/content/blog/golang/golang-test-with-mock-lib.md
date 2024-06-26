---
author: WeiChou
pubDatetime: 2023-06-20T00:00:00Z
modDatetime: 2023-06-20T00:00:00Z
title: Golang | Write Test with the Mock lib
slug: golang-test-with-mock-lib
featured: false
draft: false
# ogImage: https://res.cloudinary.com/noezectz/v1663745737/astro-paper/astropaper-x-forestry-og_kqfwp0.png
tags:
  - Golang
description:
  The mock package provides a mechanism for easily writing mock objects that can be used in place of real objects when writing test code.
---

- For more information on how to write mock code, check out the API documentation [github.com/stretchr/testify/mock](https://godoc.org/github.com/stretchr/testify/mock).
- The mockery tool to autogenerate the mock code against an interface, making using mocks much quicker. [vektra/mockery](https://github.com/vektra/mockery).

## Table of contents

## Define a Test Object
```
// Robot
type Robot interface {
	SayHi()
}

// ServiceRobot is kind of Robot can offer services
type ServiceRobot struct {
}

func (robot *ServiceRobot) SayHi() {
	fmt.Println("Hi, I'm service robot")
}

// IndustrialRobot is kind of Robot can do some jobs
type IndustrialRobot struct {
}

func (robot *IndustrialRobot) SayHi() {
	fmt.Println("Hi, I'm industrial robot")
}

func StartRobots(){
	robots := initializeRobots()
	makeRobotsSayHi(robots)
}

// initialize all robots
func initializeRobots()[]Robot{
	robots := []Robot{
		&ServiceRobot{},
		&IndustrialRobot{},
	}
	return robots
}

// makeRobotsSayHi is used for making robots say hi
func makeRobotsSayHi(robots []Robot){
	for _, robot := range robots {
		robot.SayHi()
	}
}
```

## Install mock tool

mockery provides the ability to easily **generate mocks** for **golang interfaces**. It removes the boilerplate coding required to use mocks.

`go get github.com/vektra/mockery/.../`

## Mock the interface

```
$ $GOPATH/bin/mockery -name=Robot
Generating mock for: Robot in file: mocks/Robot.go

```

```
// Code generated by mockery v1.0.0. DO NOT EDIT.

package mocks

import mock "github.com/stretchr/testify/mock"

// Robot is an autogenerated mock type for the Robot type
type Robot struct {
	mock.Mock
}

// SayHi provides a mock function with given fields:
func (_m *Robot) SayHi() {
	_m.Called()
}

```

## Write test

```
func TestMakeRobotsSayHi(t *testing.T){
	// create an instance of our test object
	mockRobotA := new(mocks.Robot)
	mockRobotB := new(mocks.Robot)

	// setup expectations
	mockRobotA.On("SayHi").Return(nil, nil)
	mockRobotB.On("SayHi").Return(nil, nil)

	robots := []Robot{
		mockRobotA,
		mockRobotB,
	}

	// Act
	makeRobotsSayHi(robots)

	// Assert that the expectations were met
	mockRobotA.AssertExpectations(t)
	mockRobotB.AssertExpectations(t)
}

```

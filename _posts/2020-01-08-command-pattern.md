---
published: true
layout: single
title: "Command Pattern"
category: book
tags: [Head First, design pattern, command pattern]
comments: false
---

> **커맨드 패턴(Command Pattern)**  
> 요구 사항을 객체로 캡슐화할 수 있다.  
> 매개변수를 써서 여러 가지 다른 요구 사항을 집어넣을 수 있다.  
> 요청 내역을 큐에 저장하거나 로그로 기록할 수도 있다.  
> 작업 취소 기능도 지원 가능하다.

### 커맨드 패턴의 정의

![다이어그램](/assets/images/command_pattern_1.PNG)

1. Client
   - 커맨드 객체(ConcreteCommand)를 생성해야 한다.
   - Receiver를 설정한다.
2. Receiver
   - 요구 사항을 수행하기 위해 어떤 일을 처리해야 하는지 알고 있는 객체
3. Command
   - 모든 커맨드 객체에서 구현해야 하는 인터페이스
   - 모든 명령은 execute() 메소드 호출을 통해 수행된다.
     - 해당 메소드에서는 receiver에 특정 작업을 처리하라는 지시를 전달한다.
     - 행동을 캡슐화한다.
       - 외부에서 볼 때는 어떤 객체가 receiver 역할을 하는지, receiver에서 실제로 어떤 일을 하는지 알 수 없다.
4. ConcreteCommand
   - 특정 행동과 receiver 사이를 연결한다.
     - 행동과 receiver에 대한 정보가 같이 들어있다.
   - invoker에서 execute() 호출을 통해 요청을 하면, receiver에 있는 메소드를 호출함으로써 작업을 처리한다.
5. Invoker
   - 명령이 들어있다.
     - 클라이언트에서 invoker 객체의 setCommand() 메소드를 호출할 때, 넘겨준 커맨드 객체를 쓰이기 전까지 invoker 객체에 보관된다.
   - execute() 메소드를 호출함으로써 커맨드 객체에게 특정 작업을 수행해 달라는 요구를 하게 된다.

### 리모컨 API 만들기

#### Command 인터페이스

```java
interface Command {
    public void execute();
}
```

#### 전등을 켜기 위한 커맨드 클래스

```java
public class LightOnCommand implements Command {
    Light light; // receiver

    public LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.on();
    }
}
```

#### 오디오를 켜기 위한 커맨드 클래스

```java
public class StereoOnWithCDCommand implements Command {
    Stereo stereo;

    public StereoOnWithCDCommand(Stereo stereo) {
        this.stereo = stereo;
    }

    public void execute() {
        stereo.on();
        stereo.setCD();
        stereo.setVolume(11);
    }
}
```

#### 버튼이 하나 밖에 없는 리모컨

- 커맨드 패턴에서 invoker에 해당하는 부분

```java
public class SimpleRemoteControl {
    Command slot;

    public SimpleRemoteControl() {}

    public void setCommand(Command command) {
        slog = command;
    }

    public void buttonWasPressed() {
        slot.execute();
    }
}
```

#### SimpleRemoteControl 테스트하기

- 커맨드 패턴에서 클라이언트에 해당하는 부분

```java
public class RemoteControlTest {
    public static void main(String[] args) {
        SimpleRemoteControl remote = new SimpleRemoteControl();
        Light light = new Light(); // receiver
        LightOnCommand lightOn = new LightOnCommand(light);

        remote.setCommand(lightOn);
        remote.buttonWasPressed();
    }
}
```

#### 슬롯이 7개인 리모컨

```java
public class RemoteControl {
    Command[] onCommands;
    Command[] offCommands;

    public RemoteControl() {
        onCommands = new Command[7];
        offCommands = new Command[7];

        Command noCommand = new NoCommand();
        for (int i = 0; i < 7; i++) {
            onCommands[i] = noCommand;
            offCommands[i] = noCommand;
        }
    }

    public void setCommand(int slot, Command onCommand, Command offCommand) {
        onCommad[slot] = onCommand;
        offCommand[slot] = offCommand;
    }

    public void onButtonWasPushed(int slot) {
        onCommands[slot].execute();
    }

    public void offButtonWasPushed(int slot) {
        offCommands[slot].execute();
    }
}
```

### UNDO 버튼을 지원하는 리모컨

1. Command 인터페이스에 undo() 메소드를 추가한다.

   - ```java
       interface Command {
           public void execute();
           public void undo();
       }
     ```

2. Command 객체에 undo() 메소드를 구현한다.

   - ```java
       public class LightOnCommand implements Command {
           Light light;

           public LightOnCommand(Light light) {
               this.light = light;
           }

           public void execute() {
               light.on();
           }

           public void undo() {
               light.off();
           }
       }
     ```

3. 마지막으로 실행된 명령어를 기록하기 위한 인스턴스 변수를 추가하고 UNDO 버튼을 누르면 기록해뒀던 커맨드 객체의 undo() 메소드를 호출한다.

   - ```java
       public class RemoteControlWithUndo {
           Command[] onCommands;
           Command[] offCommands;
           Command undoCommand;

           public RemoteControlWithUndo() {
               onCommands = new Command[7];
               offCommands = new Command[7];

               Command noCommand = new NoCommand();
               for (int i = 0; i < 7; i++) {
                   onCommands[i] = noCommand;
                   offCommands[i] = noCommand;
               }
               undoCommand = noCommand;
           }

           public void setCommand(int slot, Command onCommand, Command offCommand) {
               onCommad[slot] = onCommand;
               offCommand[slot] = offCommand;
           }

           public void onButtonWasPushed(int slot) {
               onCommands[slot].execute();
               undoCommand = onCommands[slot];
           }

           public void offButtonWasPushed(int slot) {
               offCommands[slot].execute();
               undoCommand = offCommands[slot];
           }

           public void undoButtonWasPushed() {
               undoCommand.undo();
           }
       }
     ```

### 매크로 커맨드

- 버튼 한 개만 누르면, 여러 커맨드를 실행시킬 수 있는 형태

```java
public class MacroCommand implements Command {
    Command[] commands;

    public MacroCommand(Command[] commands) {
        this.commands = commands;
    }

    public void execute() {
        for (int i = 0; i < commands.length; i++) {
            commands[i].execute();
        }
    }
}
```

```java
public class MacroCommandTest {
    public static void main(String[] args) {
        RemoteControl remote = new RemoteControl();
        Light light = new Light("Living Room");
        TV tv = new TV("Living Room");

        LightOnCommand lightOn = new LightOnCommand(light);
        TVOnCommand tvOn = new TVOnCommand(tv);
        LightOffCommand lightOff = new LightOffCommand(light);
        TVOffCommand tvOff = new TVOffCommand(tv);

        Command[] partyOn = { lightOn, tvOn };
        Command[] partyOff = { lightOff, tvOff };

        MacroCommand partyOnMacro = new MacroCommand(partyOn);
        MacroCommand partyOffMacro = new MacroCommand(partyOff);

        remote.setCommand(0, partyOnMacro, partyOffMacro);
    }
}
```

### 🤷‍ 항상 리시버가 필요한가요?

- 커맨드 객체에서 대부분의 행동을 처리해도 된다. 하지만 invoker와 receiver를 분리시키는 것이 불가능해지며 receiver를 이용해서 커맨드를 매개변수화하는 것도 할 수 없다.

### 🤷‍ UNDO 버튼을 여러 번 누를 수 있도록 하려면 어떻게 해야 하나요?

- 전에 실행한 커맨드를 stack에 집어넣으면 된다.
- 사용자가 UNDO버튼을 누를 때마다 stack 맨 위에 있는 항목을 꺼내서 undo() 메소드를 호출하기만 하면 된다.

### 커맨드 패턴 활용

1. 요청을 큐에 저장하기
   - computation의 한 부분(리시버와 일련의 행동)을 패키지로 묶어서 일급 객체 형태로 전달하는 것이 가능하다.
     - 클라이언트 애플리케이션에서 커맨드 객체를 생성하고 나서 한참 후에도 그 computation을 호출할 수 있다.
     - 스케쥴러, 스레드 풀, 작업 큐와 같은 다양한 용도에 적용할 수 있다.
2. 요청을 로그에 기록하기
   - 모든 행동을 기록해놨다가 그 애플리케이션이 다운되었을 경우, 나중에 그 행동들을 다시 호출해서 복구를 할 수 있도록 한다.
   - 커맨드 패턴에서 store()와 load() 메소드를 추가해 지원할 수 있다.
     - store: 어떤 명령어를 실행하면서 디스크에 실행 히스토리를 기록
     - load: 애플리케이션이 다운되면 기록된 커맨드 객체를 다시 로딩한다.

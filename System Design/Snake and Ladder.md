### Board.java
```java
public class Board {
    private int size;
    private int start;
    private int end;

    public Board(int size) {
        this.start = 1;
        this.end = start + size - 1;
        this.size = size;
    }
}
```

### Dice.java
```java
public class Dice {
    private int minValue;
    private int maxValue;
    private int currentValue;

    public int roll() {
        return RandomUtils.nextInt(minValue, maxValue + 1);
    }
}
```

### Game.java
```java
public class Game {
    private int numberOfSnakes;
    private int numberOfLadders;

    private Queue<Player> players;
    private List<Snake> snakes;
    private List<Ladder> ladders;

    private Board board;
    private Dice dice;

    public Game(int numberOfLadders, int numberOfSnakes, int boardSize) {
        this.numberOfLadders = numberOfLadders;
        this.numberOfSnakes = numberOfSnakes;
        this.players = new ArrayDeque<>();
        snakes = new ArrayList<>(numberOfSnakes);
        ladders = new ArrayList<>(numberOfLadders);
        board = new Board(boardSize);
        dice = new Dice(1, 6, 2);
        initBoard();
    }
}    
```

### Ladder.java
```java
public class Ladder {
    private int start;
    private int end;
}
```

### Player.java
```java
public class Player {
    private String name;
    @Setter
    private int position;
    @Setter
    private boolean won;
    public Player(String name) {
        this.name = name;
        this.position = 0;
        this.won = false;
    }
}
```

### Snake.java
```java
public class Snake {
    private int head;
    private int tail;
}
```

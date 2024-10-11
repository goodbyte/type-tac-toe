# Type Tac Toe

Tic Tac Toe game made entirely with Typescript types.

```typescript
type State = 'X' | 'O' | ' ';

type GameBoard = [
  State, State, State,
  State, State, State,
  State, State, State,
];

// 1. Check for Amount of X and O
// 2. Who is the next Player
// 3. Has one Player won?
// 4. Draw

type Extends<First, Second> = First extends Second ? true : false;

type AnyTrue<Check> = Extends<true, Check>;

type Contains<Arr extends State[], ToCheck> = Extends<ToCheck, Arr[number]>;

type Len<Arr extends State[]> = Arr['length'];

type SameLength<Arr1 extends State[], Arr2 extends State[]> = Extends<Len<Arr1>, Len<Arr2>>;

type Count<RemainingBoard extends State[], XCount extends 'X'[] = [], OCount extends 'O'[] = []> = 
  RemainingBoard extends [infer Head, ...infer Tail extends State[]]
    ? Count<Tail, Head extends 'X' ? [...XCount, 'X'] : XCount, Head extends 'O' ? [...OCount, 'O'] : OCount> 
    : [XCount, OCount];

type CorrectXO<X extends State[], O extends State[]> =
  AnyTrue<SameLength<X, O> | SameLength<X, [...O, 'O']>>;

type WhoIsNext<X extends State[], O extends State[]> = SameLength<X, O> extends true ? 'X' : 'O';

type AllSame<Arr extends [State, State, State]> = Arr extends ['X', 'X', 'X'] | ['O', 'O', 'O']
  ? true : false;

type WinCheck<Board extends GameBoard> =
  AnyTrue<AllSame<[Board[0], Board[1], Board[2]]>
  | AllSame<[Board[3], Board[4], Board[5]]>
  | AllSame<[Board[6], Board[7], Board[8]]>
  | AllSame<[Board[0], Board[3], Board[6]]>
  | AllSame<[Board[1], Board[4], Board[7]]>
  | AllSame<[Board[2], Board[5], Board[8]]>
  | AllSame<[Board[0], Board[4], Board[8]]>
  | AllSame<[Board[2], Board[4], Board[6]]>>;

type TypeTacToe<Board extends GameBoard> =
  Count<Board> extends [infer X extends State[], infer O extends State[]]
    ? CorrectXO<X, O> extends true
      ? WinCheck<Board> extends true
        ? `${WhoIsNext<X, O> extends 'X' ? 'O' : 'X'} wins`
        : Contains<Board, ' '> extends true
          ? `${WhoIsNext<X, O>} it's your turn`
          : 'Draw'
      : 'Too many Xs or Os'
    : never;

// FirstGame = 'O wins'
type FirstGame = TypeTacToe<[
  'X', 'X', 'O',
  'X', 'O', ' ',
  'O', ' ', ' ',
]>;
```

[I BUILT a GAME in TypeScript TYPES!](https://www.youtube.com/watch?v=nVGhZZbM6r4)

[Typed Rocks Channel](https://www.youtube.com/@Typed-Rocks)
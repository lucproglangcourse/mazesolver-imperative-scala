  Students build a small ASCII maze game using only built-in Scala/Java types—arrays,
  strings, integers, booleans, tuples, and Option if desired. They define procedures,
  mutate state, trace calls, and use recursion, but do not define classes, traits, or
  domain-model ADTs. That keeps the project firmly on the imperative side of your course
  boundary.

  The story: a rescue robot must explore a damaged space station, reveal rooms, collect
  energy cells, and reach an escape pod before its energy runs out.

  ###########
  #R..#....E#
  #.#.#.##..#
  #.#...C#..#
  #.#####.#.#
  #...C...#.#
  ###########

  R is the robot, C an energy cell, E the exit, # a wall, and . an unexplored floor.

  The project has four deliberately staged parts.

  1. State and procedural abstraction

  Give students a starter maze and a mutable representation such as:

  val maze: Array[Array[Char]] = ...
  val robot: Array[Int] = Array(startRow, startColumn)
  var energy = 12
  var cellsCollected = 0

  They implement procedures such as:

  def isWalkable(maze: Array[Array[Char]], row: Int, col: Int): Boolean
  def moveRobot(maze: Array[Array[Char]], robot: Array[Int], direction: Char): Boolean
  def collectCell(maze: Array[Array[Char]], robot: Array[Int]): Int
  def render(maze: Array[Array[Char]], robot: Array[Int], energy: Int): String
  def playMoves(maze: Array[Array[Char]], robot: Array[Int], moves: String): Int

  The emphasis is on contracts, local variables, procedure calls, if/while control flow,
  and clear state transitions. A supplied move string such as "RRDDLL..." makes automated
  testing straightforward; an optional interactive mode can be added later for fun.

  2. Parameter passing and aliasing lab

  Make this a required short section, not merely a discussion prompt. Students predict
  the result, run it, then explain it with a memory diagram:

  def moveNorth(position: Array[Int]): Unit =
    position(0) -= 1

  def resetPosition(position: Array[Int]): Unit =
    position = Array(1, 1) // deliberately rejected by Scala

  Then provide a valid version that only rebinds a local parameter:

  def localReset(position: Array[Int]): Unit =
    val replacement = Array(1, 1)
    // assigning position to replacement would affect only this local parameter

  Required conclusions:

  - Scala uses call by value.
  - For an array parameter, the copied value is a reference/address-like value.
  - The caller and callee can therefore reach and mutate the same array.
  - Rebinding the local parameter cannot change which array the caller’s variable refers
    to.

  - Passing an Int cannot let a procedure mutate the caller’s energy; the procedure must
    return a new value or mutate an object/array that contains it.

  For Scala by-name, a small game-relevant exercise works well:

  def repeatUntilStopped(action: => Boolean): Int =
    var attempts = 0
    while action do attempts += 1
    attempts

  Students call it with an expression that consumes energy or performs a scripted move,
  then explain why action is evaluated anew at each use. This makes it visibly unlike
  both ordinary call by value and reference passing.

  3. Recursive emergency scanner

  The robot has a scanner that reveals all reachable floor cells from its location.
  Students implement recursive flood fill:

  def revealReachable(
      maze: Array[Array[Char]],
      revealed: Array[Array[Boolean]],
      row: Int,
      col: Int
  ): Int

  The procedure has explicit base cases—out of bounds, wall, already revealed—and
  recursive calls to neighboring cells. It returns the number of newly revealed cells.

  This gives recursion a real purpose, while supporting the required discussion:

  - What is stored in one activation record?
  - Which call is the base case?
  - Draw the call stack for a supplied 4-by-4 maze and starting cell.
  - Why does marking a cell as revealed before recursive calls ensure termination?
  - What is the maximum stack depth in the worst case?

  For a stretch goal, students replace recursion with an explicit mutable stack/worklist
  and compare call-stack space with heap space. That creates a natural later bridge to
  data structures and algorithmic design without requiring it in the core assignment.

  4. Reasoning and tests

  Require unit tests for procedures, not screenshots as the primary evidence. A compact
  README should include:

  - a state-transition table for three moves;
  - one parameter-passing memory diagram;
  - a call-stack trace for the scanner;
  - a brief comparison of recursive flood fill and an iterative loop-driven task;
  - an explanation of why the project is procedural/imperative rather than OO.

  A rubric could be:

   Criterion                                                           Points
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  ━━━━━━━━
   Correct stateful movement, collection, and rendering procedures        1.0
  ──────────────────────────────────────────────────────────────────  ────────
   Procedural decomposition, contracts, and unit tests                   0.75
  ──────────────────────────────────────────────────────────────────  ────────
   Parameter-passing/aliasing analysis, including by-name                 0.5
  ──────────────────────────────────────────────────────────────────  ────────
   Correct recursive scanner plus stack trace/termination reasoning      0.75
  ──────────────────────────────────────────────────────────────────  ────────
   Clear Scala 3 style and short reflection                              0.25

  The design has a useful pedagogical feature: the student experiences why a growing
  bundle of arrays and procedures eventually becomes awkward. You do not need to solve
  that awkwardness inside this project. It becomes the motivation for the next chapter:
  “What would improve if the robot, maze, scanner, and display became domain-level
  abstractions?”

  Worked for 1m 13s · done 12:00 AM

─ Conversation recap ────────────────────────────────────────────────────────────────────

  Develop a concrete, fun imperative-focused Scala assignment; Maze Rescue, an ASCII maze
  game using built-in types, was proposed. Next: flesh out its requirements, milestones,
  and assessment criteria.

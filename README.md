java c
CSCI1120 Introduction to Computing Using C++, Fall 2024/25 
Assignment   6:   Mathable   Using   OOP
Due:   23:59, Sat   7   Dec   2024                                                                                                                                                                                                                                                                                                                              Full   marks:   100
Introduction The   objective   of   this   assignment   is   to   practice   the   use   of   inheritance   and   polymorphism.   Usage   of   strings,   vectors   and   pointers   is   also   included.   You   will   implement   the Mathable board   game   again   but   using   an OOP approach this   time.In   Assignment   4,   we   used   static   arrays   and   separate   functions   to   implement   the    board   game   with   two variant   settings,   namely classical Mathable and Mathable Junior, which   differ   in   board   size,   rack   size   and   number   of   tokens.   In   this   assignment,   you   are   to   reimplement   the   board   game   in   an   OOP   style   by developing several classes that   model the game   components.
For simplicity,
•       Only the Junior variant of the game   is   required.   Its   configuration   is   recapped   in   Table   1   below.
•       You   may assume the   board size,   rack size,   and total   number   of tokens   are   fixed   (in   Mathable.h).
•       No   need to   implement the “swap   tokens”   functionality   –   i.e.,   the   option   'S'   for   the   player’s   action does   not exist this   time.
Table   1: Game configuration   parameters   of   Mathable Junior

However, there are some   new features to   support:
•       The   number of   players can   be varied   between   2 and 4   at   the   program   start.
•       Besides   human   players, we also   implement   computer   players that   make   moves   automatically.
•       The   new game   has to support two special types   of   squares   in the   original   Mathable.   Recall   that
o Restriction    squares are      those       blue      squares       (in      Figure      1      GUI)      marked    with       an      addition,   subtraction,   multiplication   or   division   sign.   To   occupy   a   square   of   this   kind,   the   player   must   make an equation that corresponds to the   sign   of that   square.
One   of the   original   game   rules   reads   “If a player places a token on a restriction square, he may, at that moment, take an extra token (from the bag) if he so wishes. This may not be postponed to a later turn.” To   make   it simple,   we   won’t   implement this   rule.
o Bonus squares are   those    marked   with   “2x”    or   “3x”   .   A    purple   square   marked   2x   doubles   the   amount   of   point   of the   token   on that   square;   an   orange   square   marked   3x   triples   the   number   of   points.
Game Board Representation 
The   gameboard   is   represented   by   a   text-based   grid   as   shown   in Figure   1 below.

Figure   1: A GUI screenshot of the   Mathable app   versus   our   text-based   game   board   representation
Program Specification 
This   section   describes   the   starter   code,   class    hierarchy,    player   actions,   game-over   conditions,   the   TODO classes that   require your   major work,   and the   program flow.
Starter Code 
•          To   help you get   started,   you   are   provided   with   all   the   required   source   files. There   is   a   total   of    17   files   implementing this   program – 8 classes   (header and source   files)   plus the   main   client   source.
•          Your   task   is   to   fill   in   your   code   for   the   missing    parts   marked   with   TODO   comments   in   only   8   of      these files. The   last column of the Table   2   and   3   indicates whether the   file   has   some   TODO   task(s)   for   you.   Search   the   word   “TODO:”   to   locate   where   you   should   fill   in   the   missing   code.   Read   the      instructions   in   the   TODO   comment   to   know   what   is   expected   to   do.   You   may   remove   the   TODO      comments after finishing the   missing code.
Table   2:   Header files for class   definitions
File Description TODO 1 
Square.h Square interface A base class modeling a square on board N 2 
BonusSquare.h BonusSquare interface A subclass modeling a bonus square Y 3 
RestrictionSquare.h RestrictionSquare interface A subclass modeling a restriction square N 4 
Player.h Player interface An abstract base class modeling a player N 5 
Bot.h Bot interface A subclass modeling a computer player N 6 
Man.h Man interface A subclass modeling a human player N 7 
Mathable.h Mathable interface A class modeling game board and bag N 8 
RandomNumberGenerator.h RandomNumberGenerator interface A custom random number generator N 
Note:   The   constants P =   4, N =   10, R =   5,   and T =   60   are   defined   in   Mathable.h,   which   correspond   to   maximum #   players,   board size,   rack size,   bag size   (total # tokens)   respectively.
Table 3: Source files for c代 写CSCI1120 Introduction to Computing Using C++, Fall 2024/25 Assignment 6C/C++
代做程序编程语言lass   implementations   and   client   code

File Description Relation with other classes TODO 9 
Square.cpp Square implementation Base class N 10 
BonusSquare.cpp BonusSquare implementation Subclass of Square Y 11 
RestrictionSquare.cpp RestrictionSquare implementation Subclass of Square Y 12 
Player.cpp Player implementation Abstract base class Y 13 
Bot.cpp Bot implementation Subclass of Player Y 14 
Man.cpp Man implementation Subclass of Player Y 15 
Mathable.cpp Mathable implementation Player subclasses access board or bag via this object Y 16 
RandomNumberGenerator.cpp RandomNumberGenerator implementation Mathable’s setupBag() uses it for shuffling tokens N 17 
math-game.cpp Mathable game client program The main() function here Y 
•          We suggest the   following   development   order.
o    The subclasses   of   Square:   RestrictionSquare   and   BonusSquare;
o      Next   is the Mathable   class;
o      Then finish the   base classes first:   Player;
o      Followed   by subclasses of   Player:   Bot   and Man;
o      Finally, the game client   program:   math-game.cpp.
Class Hierarchy  

Figure   2: The class   inheritance   hierarchies   and   relationship   between classesThe   Mathable   class   has   a   2-D   array   of   Square   pointers,   namely   board[N][N],   which   implements   the game   board.   BonusSquare   and   RestrictionSquare   are subclasses   of Square. They override   the   virtual   function   toString()   to   return   more   specific   information   like   “3x”   or   “+”   .   The   Player   class    keeps   a    pointer,    namely    mGame,    which    points    to    a    Mathable   object.   Then    methods    in   the   Player   class   or   its   subclasses   can   make   moves,   i.e.,   placing   tokens   on   the   game   board,   by   calling   the    methods    provided    by    Mathable.    Note    that    the    Player   class    is    abstract    since    it    has    a    pure   virtual    method    called      makeMove().      You      cannot      create      objects      of    this      class.      Man and      Bot are   concrete      subclasses      of      the       Player class       modeling       a       human       player       and       a       computer       player   respectively.   They   reuse   the   parent-level   data   such   as   score   and   rack   as   well   as   methods   such   as   getScore()   and   rackSize(). They override their   parent’s virtual   makeMove()   method to   exhibit   different   behaviors   in   making   moves   –   a   human   player   enters   a   move   via   the   keyboard   whereas   a   computer   player   picks the   move that obtains   the   highest   points from all   possible   moves.Besides the classes, we also define   a   type   alias   of   int   called   Token   in   Square.hand   we tend   to   use   this   alias   for   the   type   of   variables   storing   a   token.   For   example,   a   player’s   rack   is   implemented   as   a   vector storing the tokens,   instead   of writing   vector rack,   we   would   write   vector   rack, which   looks   more   readable.
Human Player Actions: 
There are   3 options of game actions that   a   human   player   (Man)   can   choose   in   each   turn:
1. Play (P):   play a   move,   i.e.,   select   a   token   on   the   rack   to   place   at   a   specified   cell   on   the   board.
2. End (E):   end the   current   turn   (if the   player   can’t   see   any   more   moves   to   make).
3. Terminate (T):    terminate      the      game       (prematurely).    Then       players’       current    total    scores       are   compared to see who wins the   game.
You   may   refer to Assignment 4 specification for their description.
Computer Player Actions: A   computer   player   (Bot)   has   no   options   for   actions.   It   keeps   playing   all   playable   moves   in   a   greedy   manner,   i.e.,   pick   a   combination   of   a   token   on   its   rack   and   a   square   on   the   board   that   can   maximize   the   points   gained   by   the   current   move,   until   there   are   no   more   possible   moves.   Then   it   passes   the   turn to the   next   player.
Game-over Conditions: 
In this assignment, there are three game-over   conditions:
(1)         There are   no tokens   left   in the   bag   and one   player   has   used   up all tokens   on   his   rack.
(2)          Every   player   has   passed their turn to the   next since they   have   no   more   possible   moves.
•       Note: This   situation   can   happen   even   if the   bag   is   not   empty   (since the   swap-tokens   function   is   not supported   in this assignment).
•       Hint: The   possibleMoves()   method   provided   by the   Mathable   class,   to   be   discussed,   can   facilitate the detection of this   situation.
(3)         A   human   player   enters   option T to terminate the game.   (This   allows   us   to   end   a   game   earlier   as   we wish,   making   program testing   more flexible.)
Like Assignment 4, a draw game   can   happen   if   more than   one   players   attain   the   same   highest   score.



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com

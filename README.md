# CSC 249 - Project 5 - Distributed Client-Server TicTacToe Application

In this programming project, you will build a distributed client-server TicTacToe game application in Python.

As your starting point, you will study the Python source code for a simple desktop TicTacToe game - tic_tac_toe.py in this Git repository. This code was taken in its entirety from the RealPython tutorial _Build a Tic-Tac-Toe Game With Python and Tkinter_, available at this URL: https://realpython.com/tic-tac-toe-python/. You are encouraged to review the RealPython tutorial and use it to help you understand the principles underlying the implementation of the game. You should also try running the code yourself!

The learning objective of this project is to give students realistic experience and insight into the design of Application Layer message protocols that build on the reliable network communication infrastructure offered by TCP.

To achieve that learning objective, you will redesign the desktop app, converting it from its current form into a distributed client-server application. That is, you will redesign the program in a way that splits its functionality into two parts: a _TicTacToe Server_ and a _TicTacToe Client_, each of which will run as independent processes on a single computing endpoint (for solutions submitted by individual students) or on separate computing endpoints that are able to communicate with each other by virtue of being connected to the same subnet (for solutions submitted by multi-person teams).

This project will allow students to complete and submit a fully satisfactory solution that meets a defined set of requirements and is therefore eligible to receive full credit for the project. It will also offer students the opportunity to go _above and beyond_ those requirements in various ways that will (a) deepen their expertise in computer networking, (b) earn some serious bragging rights, and (c) possibly also garner some extra credit points.

Let's dig in further, shall we?

## Behavioral Requirements of the Client-Server System

In this section, we specify the desired behavior of the client-server system (the distributed TicTacToe game) in the form of a series of _requirement statements_.

A fully satisfactory client-server system must meet these behavioral requirements:

* The implementation shall comprise one client application and one server application, both of which shall be written in Python.
* Collectively, the client and server shall implement a standard TicTacToe game (3x3 board with the usual win/lose/tie rules) where all communication between client and server take place using the TCP protocol.
* The server shall be launched first (of course). It shall wait for a client connection, and once a connection is established it shall refuse additional connections.
* The server shall maintain game state, track turn taking (whether it is X's or O's turn to move), and determine when a game has been won, lost, or tied.
* The client shall display the board to a human player, accept and display player moves, and report those moves to the server.
* After a client successfully connects to the server, it shall tell the server whether the client will play X or O. This selection should be made by the user (operating the client) and communicated to the server.
* The client shall always make the first move regardless of whether it plays X or O. (In other words, the server shall alwalys wait for the client to make the first move.)
* The board displayed by the client shall always indicate whether it is the human player's turn to move, or whether it is waiting for a move from the server.
* After the client makes a move, displays that move, and reports the move to the server, it shall prevent the human player from making another move until a response is received from the server.
* Upon receipt of a move from the client, the server shall determine whether the client has won or tied the game. This information shall be communicated to the client.
* If the client receives notice from the server that the current game has been won, lost, or tied, it shall display that result and prevent additional client-side moves.
* When it is the server's turn to play, it shall play a random available move using the correct player symbol (X if the client is O, and vice versa). (Building a clever TicTacToe player is not among the learning objectives.)
* When it is the server's turn to play, the server shall wait three (3) seconds before sending its move to the client.
* The server shall communicate its model of the game state (the state of the board) to the client each time it makes a move.
* Whenever the client receives a game-state update from the server, the client shall confirm that its model of the game matches that of the server. Model mismatches shall trigger graceful recovery, resulting in the client and server sharing a common model.
* If it is the server's turn to play but the board is full (i.e., no move by the server is possible), the server shall end the game and report win, lose, or tie to the client.
* If the server makes a move that results in it winning or tieing the game, it shall report that result to the client.
* At any point during a game or at the end of a game, the client may issue a game-reset request to the server and start a new game. Such requests shall originate with the human player.
* At any point the client may issue a quit-game request to the server and gracefully terminate the connection. Such requests shall originate with the human player.
* After a client disconnects, the server shall resume waiting for a new client connection.
* At any time, the client shall be able to issue a shutdown request to the server. This request must include a non-blank password.
* If the server receives a shutdown request from the client, it shall compare the supplied password to a value that was set at server startup. If the passwords match the server shall terminate the client connection and exit gracefully.
* During operation the server shall generate a log of messages sent and received to the terminal window where it is running.

## Message Protocol and Documentation Requirements

In order for your client-server system to meet the behavioral requirements listed above, you must design and implement an Application Layer message protocol. It is this protocol - its conception, design, implementation, and effectiveness in achieving the desired behavioral requirements of the client-server system - that is central to the learning objectives of this programming project. This protocol shall consist of a collection of messages, some that will be sent from the client to the server, and others that will be sent from the server to the client. Every client-side state change or request that requires server-side action will need to be packaged in the form of a message, and vice versa.

This protocol will be unique to you and your system. It will be an artifact of a design/engineering process that you (and your team) will have to work through. It is important to understand that there is no single correct or perfect design for your message protocol. This is very much in the realm of _engineering art_. You have the chance to be creative. Obviously, your protocol must be sufficient to enable all the behavioral requirements to be satisfied. After that, it's more a matter of _efficiency_ (are your messages as long as they need to be, but no longer) and _simplicity_ (are your messages as clear, simple, and consistent as they can be). That said, don't overthink this design problem or fall into _analysis paralysis_. A multi-word message that may be verbose but gets the job done is far better than a one-word message that requires complex coding to distill the requisite content into 32 bits.

You must create a specification document (a Word or Google Doc) that describes each message in your message protocol in detail. This message protocol specification shall be included in your project deliverables.

Messages shall be specified in the standard format of sequences of 32-bit words, least significant bit at the leftmost end, as in:

Bit number:  0              15               31
Word        +----------------+----------------+
0000        |                                 |
            +----------------+----------------+
0001        |                                 |
            +----------------+----------------+
0002        |                                 |
            +----------------+----------------+
0003        |               ...               |

A good example of an individual message specification can be found in the Network+ eTextbook, where it describes the fields of a TCP Header in a pair of figures (Figure 4-3 and 4-4). Taken together, the two figures provide enough detail to allow a competent programmer not only to understand the use of each message field but also to write code that will generate messages in the proper format.

## Intermediate Milestones and Deadlines

Your work on this project will begin in Week 9 of the course. Over the following five weeks you will prepare and deliver four weekly milestone products that demonstrate progress and enable your success. Weekly milestone products will be submitted on Mondays by the regular 11:59PM deadline, and each will be graded on a 10-point scale as if it were a homework assignment. Your final deliverable will be due in the fifth week, on the last day of class.

These are the milestones you must meet:

* Mon Nov 13: Partial draft of message protocol specification with at least three messages sketched for both the client and the server
* Mon Nov 20: Second draft of message protocol specification; initial split of code into client and server; 
* Mon Nov 27:
* Mon Dec 4:

## Final Deliverables and Deadline


## Grading Rubric


## Going Above and Beyond

Integrate the app server approach



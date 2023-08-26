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

1. The implementation shall comprise one client application and one server application. The client and server shall implement a standard TicTacToe game (3x3 board with the usual win/lose/tie rules) where all communication between client and server take place using the TCP protocol.
2. The server shall be launched first (of course). It shall wait for a client connection, and once a connection is established it shall refuse additional connections.
3. The server shall maintain game state, track turn taking (whether it is X's or O's turn to move), and determine when a game has been won, lost, or tied.
4. The client shall display the board to a human player, accept and display player moves, and report those moves to the server. When the client receives a move from the server, it shall update the board display accordingly.
5. After a client successfully connects to the server, it shall tell the server whether the client will play X or O. This selection should be made by the user (operating the client).
6. The client shall always make the first move regardless of whether it plays X or O. (In other words, the server shall alwalys wait for the client to make the first move.)
7. The board displayed by the client shall always indicate whether it is the human player's turn to move, or whether it is waiting for a move from the server.
8. After the client makes a move, displays that move, and reports the move to the server, it shall prevent the human player from making another move until a response is received from the server.
9. Upon receipt of a move from the client, the server shall determine whether the client has won or tied the game. This information shall be communicated to the client.
10. If the client receives notice from the server that the current game has been won, lost, or tied, it shall display that result and prevent additional client-side moves.
11. When it is the server's turn to play, it shall play a random available move using the correct player symbol (X if the client is O, and vice versa). The server shall wait three (3) seconds before sending its move to the client.
12. The server shall communicate its model of the game state (the state of the board) to the client each time it makes a move.
13. When the client receives a game-state update from the server, the client shall confirm that its model of the game matches that of the server.
14. If it is the server's turn to play but the board is full (i.e., no move by the server is possible), the server shall end the game and report win, lose, or tie to the client.
15. If the server makes a move that results in it winning or tieing the game, it shall report that result to the client.
16. At any point during a game or at the end of a game, the client may issue a game-reset request to the server and start a new game.
17. At any point the client may issue a quit-game request to the server and gracefully terminate the connection.
18. After a client disconnects, the server shall resume waiting for a new client connection.

## Message Protocol and Documentation Requirements

In order for your client-server system to meet the behavioral requirements listed above, you must design and implement an Application Layer message protocol. It is this protocol - its conception, design, implementation, and effectiveness in achieving the desired behavioral requirements of the client-server system - that is central to the learning objectives of this programming project. This protocol shall consist of a collection of messages, some that will be sent from the client to the server, and others that will be sent from the server to the client. Every client-side state change or request that requires server-side action will need to be packaged in the form of a message, and vice versa.

This protocol will be unique to you and your system. It will be an artifact of a design/engineering process that you (and your team) will have to work through. It is important to understand that there is no single correct or perfect design for your message protocol. This is very much in the realm of _engineering art_. You have the chance to be creative. Obviously, your protocol must be sufficient to enable all the behavioral requirements to be satisfied. After that, it's more a matter of _efficiency_ (are your messages as long as they need to be, but no longer) and _simplicity_ (are your messages as clear, simple, and consistent as they can be). That said, don't overthink this design problem or fall into _analysis paralysis_. A multi-word message that may be verbose but gets the job done is far better than a one-word message that requires complex coding to distill the requisite content into 32 bits.

You must create a specification document (a Word or Google Doc) that describes each message in your message protocol in detail. This message protocol specification shall be included in your project deliverables.

Messages shall be specified in the standard format of sequences of 32-bit words, least significant bit at the leftmost end. A good example of an individual message specification can be found in the Network+ eTextbook, where it describes the fields of a TCP Header in a pair of figures (Figure 4-3 and 4-4). Taken together, the two figures provide enough detail to allow a competent programmer not only to understand the use of each message field but also to write code that will generate messages in the proper format.

## Intermediate Milestones and Deadlines

Your work on this project will begin in Week 9 of the course. During weeks 10 through 13 you will prepare and deliver weekly milestone products that demonstrate progress and enable your overall success. Weekly milestone products will be submitted on Mondays by the regular 11:59PM deadline, and each will be graded on a 10-point scale as if it were a homework assignment. Your final deliverable will be due in the fifth week, on the last day of class.

These are the milestones you must meet:

* Mon Nov 13: Preliminary draft of message protocol specification with at least three messages sketched for both the client and the server.
* Mon Nov 20: Second draft of message protocol specification; initial split of code into client and server. Client and server able to establish a network connection. Client able to send a string to the server.
* Mon Nov 27: Third draft of message protocol specification with all messages defined. Each message specification must indicate which behavioral requirement it supports. Client and server able to exchange at least three messages each from the protocol specification.
* Mon Dec 4: Near final draft of message protocol specification with all messages fully defined and linked to behavioral requirements. Client and server implementations shall demonstrably satisfy at least 12 of the behavioral requirements.

All milestone products shall be submitted via Gradescope, accessed using links posted on the course Moodle site for the week in which they are due.

## Final Deliverables and Deadline

Final deliverables for this project are due on the last day of class, Thursday December 13, at 11:59PM.

Your final deliverables must include:

* Final draft of message protocol specification. All messages you have implemented are clearly defined and linked to behavioral requirements.
* Python code for client and server components.
* A narrated video demonstration of your client-server application in operation. Your video should show all behavioral requirements that your implementation satisfies, and these should be noted in the audio narration.

## Grading Rubric

This project counts for 20 points (out of 100) towards your final course grade.

Of these 20 points, 10 points will be based on an evaluation your message specification. A fully satisfactory message specification shall satisfy these criteria:

* All messages are defined in a consistent format.
* The set of defined messages are sufficient to support the behavioral requirements of the client-server application.
* Each message specification indicates the behavioral requirement(s) it supports.
* Each message specifies its fields, their order relative to the start of the message, and their size (in bits). (Compare to Network+ Figure 4-3.)
* The use of each message field is defined. (Compare to Network+ Figure 4-4.)

The remaining 10 points will be based on an evaluation of your client-server implementation. Your implementation serves as running code which, by demonstrating that all the behavioral requirements are satisfied, proves the adequacy and completeness of your message protocol. Your implementation will be independently tested against the behavioral requirements, and will accumulate points based on how many of the requirements are satisfied.

## Going Above and Beyond

There are various ways for students to exceed the design requirements in ways that expand their knowledge of computer networking principles. Successful "above and beyond" efforts shall be rewarded with extra credit points that could potentially raise one's project score beyond 20 points, offsetting points lost during the semester on homework assignments or other projects. Choose wisely! Avoid taking on anything extra that might put at risk your ability to meet the defined requirements and expectations..

Here are some options to consider:

* Go beyond exchange of formatted strings by integrating the _Application Client and Server_ approach described in the RealPython socket programming tutorial [https://realpython.com/python-sockets/]
* Instead of one client to one server, extend the server and client so that the server can manage a TicTacToe game between two different players running separate clients
* Extend the server so that it can support more than one game at a time. That is, multiple clients where the server is playing one side of the game with each client.
* Pitch me an idea of your own!



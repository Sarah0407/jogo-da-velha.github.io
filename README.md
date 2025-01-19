import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Jogo da Velha',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: JogoDaVelha(),
    );
  }
}

class JogoDaVelha extends StatefulWidget {
  @override
  _JogoDaVelhaState createState() => _JogoDaVelhaState();
}

class _JogoDaVelhaState extends State<JogoDaVelha> {
  List<String> board = ['', '', '', '', '', '', '', '', '']; // Tabuleiro
  String currentPlayer = 'X'; // Jogador atual
  String winner = ''; // Vencedor
  
  // Função para jogar
  void play(int index) {
    setState(() {
      if (board[index] == '' && winner == '') {
        board[index] = currentPlayer;
        if (checkWinner()) {
          winner = currentPlayer;
        } else {
          currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
        }
      }
    });
  }

  // Verificar se há um vencedor
  bool checkWinner() {
    List<List<int>> winningCombinations = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6]
    ];

    for (var combination in winningCombinations) {
      if (board[combination[0]] == board[combination[1]] &&
          board[combination[1]] == board[combination[2]] &&
          board[combination[0]] != '') {
        return true;
      }
    }
    return false;
  }

  // Função para reiniciar o jogo
  void restartGame() {
    setState(() {
      board = ['', '', '', '', '', '', '', '', ''];
      winner = '';
      currentPlayer = 'X';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Jogo da Velha'),
      ),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          GridView.builder(
            shrinkWrap: true,
            gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 3,
              crossAxisSpacing: 4,
              mainAxisSpacing: 4,
            ),
            itemCount: 9,
            itemBuilder: (context, index) {
              return GestureDetector(
                onTap: () => play(index),
                child: Container(
                  alignment: Alignment.center,
                  decoration: BoxDecoration(
                    color: Colors.blueAccent,
                    borderRadius: BorderRadius.circular(10),
                  ),
                  child: Text(
                    board[index],
                    style: TextStyle(
                      fontSize: 40,
                      color: Colors.white,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              );
            },
          ),
          SizedBox(height: 20),
          if (winner != '')
            Text(
              'Jogador $winner venceu!',
              style: TextStyle(fontSize: 30, fontWeight: FontWeight.bold),
            ),
          SizedBox(height: 20),
          ElevatedButton(
            onPressed: restartGame,
            child: Text('Reiniciar Jogo'),
          ),
        ],
      ),
    );
  }
}

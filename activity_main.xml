package com.codinginflow.tictactoetest;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

//    create button variables --> two dimensional array: 3 rows & 3 column
    private Button[][] buttons = new Button[3][3];
//    when the game start the player 1 (x) will start
    private boolean player1Turn = true;
//    counter that will count the round
    private int roundCount;
//    int for both player 1 and 2
    private int player1Points;
    private int player2Points;
// these views will display the points of both players
    private TextView textViewPlayer1;
    private TextView textViewPlayer2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

//        display textViews of player 1 and 2 current points
        textViewPlayer1 = findViewById(R.id.text_view_p1);
        textViewPlayer2 = findViewById(R.id.text_view_p2);

//        display buttons(playing table) dynamically. Use loop to grab & append id of each button based on row and column (ref.main.xml)
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
//               create variable to capture the button id
                String buttonID = "button_" + i + j;
//                create variable to show view for each button instead of individually using findViewByID
                int resID = getResources().getIdentifier(buttonID, "id", getPackageName());
//                assign each button with resID to get the ref view. the button[i][j] will fit perfectly with the buttonID above
                buttons[i][j] = findViewById(resID);
//                to capture player's interaction, setOnClickListener. "this" refers to main activity; it will prompt us to implement the method (after extends AppCompat when initiate the class)
                buttons[i][j].setOnClickListener(this);

            }
        }

//        display reset button
        Button buttonReset = findViewById(R.id.button_reset);
        buttonReset.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                resetGame();
            }
        });
    }

    @Override
//    the logic is here! These steps are the flow of the game. Methods will be defined below them
    public void onClick(View v) {
//        check if a view(v) or a cell in the playing table is used (not contain empty string), if so, return the view
        if (!((Button) v).getText().toString().equals("")) {
            return;
        }

//  if a view is empty and it's player1Turn, setText "X", otherwise, set as "O"
        if (player1Turn) {
            ((Button) v).setText("X");
        } else {
            ((Button) v).setText("O");
        }

//  increment roundcounts
        roundCount++;

//   if someone wins, then...
        if (checkForWin()) {
            if (player1Turn) {
                player1Wins();
            } else {
                player2Wins();
            }
        } else if (roundCount == 9) {
            draw();
        } else {
//            if no one wins and no draw, switch turn
            player1Turn = !player1Turn;
        }
    }

//    to check if a player wins, check each row and column
    private boolean checkForWin() {
//        first, create a new String variable field that represents X or O in each cell
        String[][] field = new String[3][3];

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
//       go through all buttons and save into String field because we need to compare each string in each button to find the winner
                field[i][j] = buttons[i][j].getText().toString();
            }
        }

//        check all rows, columns, and diagnol, if the first equals to the second and third and not empty, return true
        for (int i = 0; i < 3; i++) {
            if (field[i][0].equals(field[i][1]) && field[i][0].equals(field[i][2]) && !field[i][0].equals("")) {
                return true;
            }
        }
        for (int i = 0; i < 3; i++) {
            if (field[0][i].equals(field[1][i]) && field[0][i].equals(field[2][i]) && !field[0][i].equals("")) {
                return true;
            }
        }

        if (field[0][0].equals(field[1][1]) && field[0][0].equals(field[2][2]) && !field[0][0].equals("")) {
            return true;
        }

        if (field[0][2].equals(field[1][1]) && field[0][2].equals(field[2][0]) && !field[0][2].equals("")) {
            return true;
        }

//        if none of above is the case, then...
        return false;
    }

    private void player1Wins() {
//        increment the points, toast, update the points, and reset the game. same with player 2
        player1Points++;
        Toast.makeText(this, "Player 1 wins!", Toast.LENGTH_SHORT).show();
        updatePointsText();
        resetBoard();
    }

    private void player2Wins() {
        player2Points++;
        Toast.makeText(this, "Player 2 wins!", Toast.LENGTH_SHORT).show();
        updatePointsText();
        resetBoard();
    }

    private void draw() {
//        no need to update the points because nobody wins. Only reset the board
        Toast.makeText(this, "Draw!", Toast.LENGTH_SHORT).show();
        resetBoard();
    }

    private void updatePointsText() {
        textViewPlayer1.setText("Player 1: " + player1Points);
        textViewPlayer2.setText("Player 2: " + player2Points);
    }

    private void resetBoard() {
//        reset all the cells in the board to empty String, including roundCount and play turn
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                buttons[i][j].setText("");
            }
        }
        roundCount = 0;
        player1Turn = true;
    }

    private void resetGame() {
        player1Points = 0;
        player2Points = 0;
        updatePointsText();
//        reset all buttons, too
        resetBoard();
    }

//    to save state when rotate the device
    @Override
    protected void onSaveInstanceState(@Nullable Bundle outState) {
        super.onSaveInstanceState(outState);

// when rotate, the following values will be stored in outState bundle
        outState.putInt("roundCount", roundCount);
        outState.putInt("player1Points", player1Points);
        outState.putInt("player2Points", player2Points);
        outState.putBoolean("player1Turn", player1Turn);
    }


    //    to restore the values back to the device after rotate
    @Override
    protected void onRestoreInstanceState(@NonNull Bundle savedInstanceState) {
        super.onRestoreInstanceState(savedInstanceState);

//        savedInstanceState bundle will restore all the values as below:
        roundCount = savedInstanceState.getInt("roundCount");
        player1Points = savedInstanceState.getInt("player1Points");
        player2Points = savedInstanceState.getInt("player2Points");
        player1Turn = savedInstanceState.getBoolean("player1Turn");

    }

    // to prevent game reset when press back button/ user navigate away from the app

    public void onBackPressed() {
        moveTaskToBack(false);
    }
}

import javax.swing.; import java.awt.; import java.awt.event.ActionEvent; import java.awt.event.ActionListener;

public class TicTacToeGUI extends JFrame implements ActionListener {  JButton[] buttons = new JButton[9];
boolean playerX = true;
JLabel statusLabel;

public TicTacToeGUI() {

    setTitle("Tic Tac Toe");
    setSize(400, 450);
    setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    setLocationRelativeTo(null);
    setLayout(new BorderLayout());

    // Status Label
    statusLabel = new JLabel("Player X Turn");
    statusLabel.setFont(new Font("Arial", Font.BOLD, 20));
    statusLabel.setHorizontalAlignment(JLabel.CENTER);
    add(statusLabel, BorderLayout.NORTH);

    // Game Panel
    JPanel panel = new JPanel();
    panel.setLayout(new GridLayout(3, 3));

    // Create Buttons
    for (int i = 0; i < 9; i++) {
        buttons[i] = new JButton("");
        buttons[i].setFont(new Font("Arial", Font.BOLD, 50));
        buttons[i].addActionListener(this);
        panel.add(buttons[i]);
    }

    add(panel, BorderLayout.CENTER);

    // Restart Button
    JButton restartButton = new JButton("Restart Game");
    restartButton.setFont(new Font("Arial", Font.BOLD, 18));

    restartButton.addActionListener(e -> resetGame());

    add(restartButton, BorderLayout.SOUTH);

    setVisible(true);
}

@Override
public void actionPerformed(ActionEvent e) {

    JButton clickedButton = (JButton) e.getSource();

    // Prevent overwriting
    if (!clickedButton.getText().equals("")) {
        return;
    }

    // Set X or O
    if (playerX) {
        clickedButton.setText("X");
        statusLabel.setText("Player O Turn");
    } else {
        clickedButton.setText("O");
        statusLabel.setText("Player X Turn");
    }

    // Check winner
    if (checkWinner()) {
        String winner = playerX ? "Player X Wins!" : "Player O Wins!";
        statusLabel.setText(winner);

        JOptionPane.showMessageDialog(this, winner);

        disableButtons();
    }
    // Check draw
    else if (isDraw()) {
        statusLabel.setText("It's a Draw!");
        JOptionPane.showMessageDialog(this, "It's a Draw!");
    }
    // Switch player
    else {
        playerX = !playerX;
    }
}

// Check Winner
private boolean checkWinner() {

    String[][] wins = {
            {getText(0), getText(1), getText(2)},
            {getText(3), getText(4), getText(5)},
            {getText(6), getText(7), getText(8)},
            {getText(0), getText(3), getText(6)},
            {getText(1), getText(4), getText(7)},
            {getText(2), getText(5), getText(8)},
            {getText(0), getText(4), getText(8)},
            {getText(2), getText(4), getText(6)}
    };

    for (String[] line : wins) {
        if (!line[0].equals("") &&
                line[0].equals(line[1]) &&
                line[1].equals(line[2])) {
            return true;
        }
    }

    return false;
}

// Check Draw
private boolean isDraw() {
    for (JButton button : buttons) {
        if (button.getText().equals("")) {
            return false;
        }
    }
    return true;
}

// Disable buttons after game over
private void disableButtons() {
    for (JButton button : buttons) {
        button.setEnabled(false);
    }
}

// Reset Game
private void resetGame() {
    for (JButton button : buttons) {
        button.setText("");
        button.setEnabled(true);
    }

    playerX = true;
    statusLabel.setText("Player X Turn");
}

// Get Button Text
private String getText(int index) {
    return buttons[index].getText();
}

// Main Method
public static void main(String[] args) {
    new TicTacToeGUI();
}

import java.util.*;

public class WordScrambleGame {
    static String[] wordList = {
        "LOVEUSTC", "Computer", "Mouse", "Rose", "Java",
        "Object", "Problem", "Flower", "Bird", "Project"
    };

    static Scanner scanner = new Scanner(System.in);
    static final int TIME_LIMIT = 15; // seconds per turn

    public static void main(String[] args) {
        System.out.println("Welcome to the Word Scramble Game!");
        System.out.print("Enter number of players: ");
        int numPlayers = Integer.parseInt(scanner.nextLine());

        String[] playerNames = new String[numPlayers];
        int[] scores = new int[numPlayers];

        // Get player names
        for (int i = 0; i < numPlayers; i++) {
            System.out.print("Enter name of Player " + (i + 1) + ": ");
            playerNames[i] = scanner.nextLine();
        }

        // Game loop
        for (String word : wordList) {
            String scrambled = scrambleWord(word);
            for (int i = 0; i < numPlayers; i++) {
                System.out.println("\n" + playerNames[i] + "'s turn");
                System.out.println("Scrambled Word: " + scrambled);
                System.out.println("You have " + TIME_LIMIT + " seconds to guess.");

                long startTime = System.currentTimeMillis();

                System.out.print("Your Guess: ");
                String guess = scanner.nextLine().trim();

                long endTime = System.currentTimeMillis();
                long timeTaken = (endTime - startTime) / 1000;

                if (timeTaken > TIME_LIMIT) {
                    System.out.println("Time's up! You took too long.");
                } else if (guess.equalsIgnoreCase(word)) {
                    System.out.println("Correct!");
                    scores[i]++;
                } else {
                    System.out.println("Wrong! The correct word was: " + word);
                }
            }
        }

        // Display scores
        System.out.println("\n--- Game Over ---");
        int highestScore = -1;
        String winner = "";
        for (int i = 0; i < numPlayers; i++) {
            System.out.println(playerNames[i] + "'s score: " + scores[i]);
            if (scores[i] > highestScore) {
                highestScore = scores[i];
                winner = playerNames[i];
            } else if (scores[i] == highestScore && !winner.contains(playerNames[i])) {
                winner += " & " + playerNames[i]; // handle tie
            }
        }

        System.out.println("\nWinner: " + winner + " with " + highestScore + " points!");
    }

    // Scrambles a word by shuffling its letters
    public static String scrambleWord(String word) {
        List<Character> letters = new ArrayList<>();
        for (char c : word.toCharArray()) {
            letters.add(c);
        }
        Collections.shuffle(letters);

        StringBuilder scrambled = new StringBuilder();
        for (char c : letters) {
            scrambled.append(c);
        }
        return scrambled.toString();
    }
}

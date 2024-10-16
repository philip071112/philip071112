import random

# Define card values
card_values = {
    '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9,
    '10': 10, 'J': 10, 'Q': 10, 'K': 10, 'A': 11
}

# Function to calculate the score
def calculate_score(hand):
    score = sum(card_values[card] for card in hand)
    # Adjust for Aces
    aces = hand.count('A')
    while score > 21 and aces:
        score -= 10
        aces -= 1
    return score

# Function to display hands
def display_hands(player_hand, dealer_hand, show_dealer_card=False):
    print(f"Player's hand: {player_hand} | Score: {calculate_score(player_hand)}")
    if show_dealer_card:
        print(f"Dealer's hand: {dealer_hand} | Score: {calculate_score(dealer_hand)}")
    else:
        print(f"Dealer's hand: [{dealer_hand[0]}, '?']")

# Main game function
def play_blackjack():
    # Create a deck of cards
    deck = list(card_values.keys()) * 4
    random.shuffle(deck)

    # Initial hands
    player_hand = [deck.pop(), deck.pop()]
    dealer_hand = [deck.pop(), deck.pop()]

    # Player's turn
    while True:
        display_hands(player_hand, dealer_hand)
        if calculate_score(player_hand) == 21:
            print("Blackjack! You win!")
            return
        elif calculate_score(player_hand) > 21:
            print("Bust! You lose!")
            return
        
        action = input("Do you want to 'hit' or 'stand'? ").lower()
        if action == 'hit':
            player_hand.append(deck.pop())
        elif action == 'stand':
            break

    # Dealer's turn
    while calculate_score(dealer_hand) < 17:
        dealer_hand.append(deck.pop())

    # Show final hands
    display_hands(player_hand, dealer_hand, show_dealer_card=True)

    # Determine the winner
    player_score = calculate_score(player_hand)
    dealer_score = calculate_score(dealer_hand)

    if dealer_score > 21 or player_score > dealer_score:
        print("You win!")
    elif player_score < dealer_score:
        print("You lose!")
    else:
        print("It's a tie!")

# Start the game
if __name__ == "__main__":
    play_blackjack()

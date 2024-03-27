# NFT-based-Event-Ticketing
Créez une plateforme de billetterie basée sur les NFT qui élimine la fraude et permet aux fans de revendre en toute transparence leurs billets sur le marché secondaire.
from web3 import Web3
import json

# Connect to Ethereum blockchain (this is a testnet example)
w3 = Web3(Web3.HTTPProvider('https://rinkeby.infura.io/v3/your_infura_project_id'))

# Check connection
if not w3.isConnected():
    print("Failed to connect to the Ethereum network!")
    exit()

# Load your smart contract
contract_address = '0xYourContractAddressHere'
with open('YourContractABI.json', 'r') as abi_definition:
    contract_abi = json.load(abi_definition)
ticket_contract = w3.eth.contract(address=contract_address, abi=contract_abi)

# Function to mint a new ticket (NFT)
def mint_ticket(buyer_address, event_id, ticket_price):
    """
    Mint a new ticket as an NFT for the buyer.
    :param buyer_address: Address of the ticket buyer.
    :param event_id: ID of the event for which the ticket is minted.
    :param ticket_price: Price of the ticket.
    """
    # Assume the function in your smart contract for minting a ticket is called "mintTicket"
    tx_hash = ticket_contract.functions.mintTicket(buyer_address, event_id, ticket_price).transact({'from': w3.eth.defaultAccount})
    receipt = w3.eth.wait_for_transaction_receipt(tx_hash)
    print(f"Ticket minted. Transaction receipt: {receipt}")

# Example usage
buyer_address = '0xBuyerEthereumAddressHere'
event_id = 123  # Example event ID
ticket_price = w3.toWei(0.1, 'ether')  # Ticket price in Ether

# Setting up a default account - in a real application, you would use the account of the ticket issuer
w3.eth.defaultAccount = w3.eth.accounts[0]

# Mint a ticket
mint_ticket(buyer_address, event_id, ticket_price)

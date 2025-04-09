API Design Document for Web3 Government Structure Navigator (Low Availability)

Project: Decentralized Government Structure Navigator
Prepared by: Nwoba Arinze
Track: Backend
Group: 7
Date: 08-04-2025
Base URL:



1. Overview
This document outlines the API design for the Web3 version of the Government Structure Navigator feature in the CivicLink platform. The aim is to provide an immutable, decentralized solution for storing and verifying government structure data using smart contracts while operating in an environment with low availability (e.g., intermittent connectivity or unreliable networks).

2. System Requirements
Web3 Components: The data will be stored in smart contracts on the blockchain, ensuring transparency and verification of government structures.

Low Availability: The system will be designed to function in environments with intermittent or low connectivity. Offline functionality will be minimal, relying on cached blockchain data where necessary.

Smart Contracts: Use Ethereum-based or other blockchain smart contracts to store verified government structure data and provide access to it.

Data Integrity: Smart contracts will ensure data is tamper-proof, transparent, and auditable.

3. API Endpoints
3.1 Authentication and Wallet Connection Endpoints
POST /auth/connect
Description: Connect the user's Web3 wallet (MetaMask, WalletConnect, etc.) to authenticate.

Request Body:

json
Copy
{
  "walletProvider": "MetaMask",
  "walletAddress": "0x...your_wallet_address"
}
Response:

json
Copy
{
  "status": "connected",
  "walletAddress": "0x...your_wallet_address"
}
POST /auth/logout
Description: Disconnect the user's Web3 wallet.

Request Body:

json
Copy
{
  "walletAddress": "0x...your_wallet_address"
}
Response:

json
Copy
{
  "status": "disconnected"
}
3.2 Blockchain Data Retrieval Endpoints
GET /blockchain/governmentStructure
Description: Retrieves the verified government structure data directly from the blockchain.

Since the data is stored in a smart contract, this API endpoint fetches it and ensures users get the most up-to-date and verified information available.

Response:

json
Copy
{
  "status": "success",
  "structure": [
    {
      "role": "President",
      "responsibilities": "Executive power, policy-making",
      "location": "National",
      "officials": [
        {
          "name": Bola Ahmed Tinubu",
          "contact": "president@nigeria.gov",
          "socialScore": 80
        }
      ]
    },
    {
      "role": "Governor",
      "responsibilities": "State leadership",
      "location": "Cross-river",
      "officials": [
        {
          "name": "Bassey Otu",
          "contact": "governor@cross-river.gov",
          "socialScore": 90
        }
      ]
    }
  ]
}
GET /blockchain/verifyOfficial
Description: Verifies the official's information against the blockchain’s smart contract.

This is important to ensure that the data fetched is consistent and verified from the decentralized source.

Request Parameters:

officialId (string): The unique identifier for the official.

role (string): The role of the official (e.g., "Governor").

location (string): The location or state of the official (e.g., "imo").

Response:

json
Copy
{
  "status": "verified",
  "official": {
    "name": "Hope Uzodimma",
    "role": "Governor",
    "location": "imo",
    "contact": "governor@imo.gov",
    "socialScore": 85
  }
}
3.3 Blockchain Syncing and Smart Contract Management
GET /blockchain/syncStatus
Description: Provides the current status of blockchain synchronization.

Useful for determining whether the data is fully synchronized and available, especially in low-availability environments.

Response:

json
Copy
{
  "status": "sync_in_progress",
  "blockchainTransactionHash": "0x...transaction_hash"
}
POST /blockchain/syncData
Description: Syncs updated government structure data to the blockchain via smart contracts.

This would allow authorized users (e.g., government officials) to update the records directly onto the blockchain, ensuring consistency across all nodes.

Request Body:

json
Copy
{
  "role": "Governor",
  "location": "Enugu",
  "official": {
    "name": "Peter Mba",
    "contact": "governor@enugu.gov",
    "socialScore": 85
  }
}
Response:

json
Copy
{
  "status": "data_synced",
  "transactionHash": "0x...transaction_hash"
}
3.4 Offline Data Handling
GET /cache/governmentStructure
Description: Retrieves the cached government structure data in case the user is offline or the blockchain is unavailable.

This is useful for low-availability situations where data needs to be available even when the blockchain is not accessible.

Response:

json
Copy
{
  "status": "cached",
  "structure": [
    {
      "role": "President",
      "responsibilities": "Executive power, policy-making",
      "location": "National",
      "officials": [
        {
          "name": “Bola Ahmed Tinubu",
          "contact": "president@nigeria.gov",
          "socialScore": 80
        }
      ]
    }
  ]
}
POST /cache/refresh
Description: Refreshes the cached data with the latest blockchain data once the network is back online.

In low-availability environments, this is used when connectivity returns to update the offline cache with the most recent data.

Response:

json
Copy
{
  "status": "cache_refreshed",
  "lastUpdated": "2025-04-09T10:00:00Z"
}
3.5 Error Handling for Low Availability
Error Codes:
400 - Bad Request: Missing or invalid parameters (e.g., incorrect wallet address).

401 - Unauthorized: Invalid or expired wallet authentication.

404 - Not Found: Data not found (e.g., no government structure for given location).

503 - Service Unavailable: Blockchain or smart contract unreachable, typically due to low availability or network issues.

Error Responses:
json
Copy
{
  "error": "Service Unavailable",
  "message": "Unable to connect to the blockchain at the moment. Please try again later."
}

4. Security Considerations

Smart Contract Security: All smart contracts used for storing government data will be audited for security to prevent data manipulation.

Encryption: Sensitive data should always be transmitted securely using HTTPS, and personal information will be encrypted in transit.

Wallet Authentication: Only authenticated users with a valid Web3 wallet (MetaMask, etc.) will be allowed to make changes or verify data.

5. Rate Limiting
Since this version of the app is expected to run in low-availability environments, rate-limiting will be applied sparingly:

Standard Rate Limit: 50 requests per minute.

Burst Limit: Up to 100 requests for specific actions like data sync or cache refresh.

6. Testing and Monitoring

Testing: Test blockchain synchronization under low-connectivity conditions.

Run tests to validate data caching and fallback mechanisms when the blockchain is unavailable.

Monitoring:

Use tools to track smart contract interactions, cache refresh rates, and error logs for low-availability conditions.

Monitor the status of blockchain nodes to check for downtime or network interruptions.




7. Conclusion

The Web3 version of the Government Structure Navigator will allow for a decentralized, immutable source of verified government data. It is designed to function even in environments with low availability by providing offline caching capabilities and syncing with the blockchain when connectivity is restored. This approach will ensure that citizens can access government structure information with transparency and accountability, even in low-connectivity areas.


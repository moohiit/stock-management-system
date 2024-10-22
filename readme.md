
# Stock Management System - API Development and MongoDB Integration

This project provides a system for uploading and processing stock data (in CSV format), validating the data, storing it in MongoDB, and offering specific data retrieval APIs with calculations like highest volume, average close, and average VWAP.

## Table of Contents
1. [Features](#features)
2. [Technologies Used](#technologies-used)
3. [Installation](#installation)
4. [API Endpoints](#api-endpoints)
   - [Upload CSV and Data Validation](#upload-csv-and-data-validation)
   - [Calculations and Data Retrieval](#calculations-and-data-retrieval)
5. [Database Structure](#database-structure)
6. [Testing and Documentation](#testing-and-documentation)
7. [Postman Collection](#postman-collection)
8. [Unit Tests](#unit-tests)

---

## Features

1. **Upload CSV File**: Upload stock data in CSV format.
2. **Data Validation**: Validate the format and content of the CSV file.
3. **Store in MongoDB**: Save validated records in a MongoDB collection.
4. **Perform Calculations**: Retrieve stock data and perform calculations like:
   - Highest Volume
   - Average Close Price
   - Average VWAP
5. **API Queries**: Filter records by date range or symbol.

## Technologies Used

- **Node.js**
- **Express.js**
- **MongoDB & Mongoose**
- **Multer** for file uploads
- **CSV-Parser** for parsing CSV files
- **Postman** for API testing

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/moohiit/stock-management-system.git
   cd stock-management-system
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Create a `.env` file in the root directory and add your MongoDB URI:

   ```env
   MONGODB_URI=mongodb://localhost:27017/stockDB
   PORT=5000
   ```

4. Start the application:

   ```bash
   npm start
   ```

## API Endpoints

### 1. Upload CSV and Data Validation

**Endpoint**: `/upload`  
**Method**: `POST`  
**Headers**:  
- `Content-Type: multipart/form-data`

**Description**: This endpoint accepts a CSV file, validates its structure and content, and uploads valid data to MongoDB.

**Expected CSV Columns**:
- Date, Symbol, Series, Prev Close, Open, High, Low, Last, Close, VWAP, Volume, Turnover, Trades, Deliverable, %Deliverable

**Validation Rules**:
- **Date**: Must be in `YYYY-MM-DD` format.
- **Numerical Fields**: Fields like `Prev Close`, `Open`, `High`, `Low`, etc., must be numbers.

**Response**:
- **Success**:
  ```json
  {
    "message": "Data successfully saved in the database.",
    "success": true,
    "totalRecords": 5306,
    "successfulRecords": 4797,
    "failedRecordsCount": 509,
    "failedRecords": [
        {
            "symbol": "BPCL",
            "date": "2000-01-03",
            "reason": "Some column records are missing"
        },
        {
            "symbol": "BPCL",
            "date": "2000-01-10",
            "reason": "Some column records are missing"
        },
        {
            "symbol": "BPCL",
            "date": "2000-01-11",
            "reason": "Some column records are missing"
        },
        {
            "symbol": "BPCL",
            "date": "2000-01-12",
            "reason": "Some column records are missing"
        },
        ... so on
      ]
  }
  ```
- **Error**:
  ```json
  {
    message: "Error processing CSV file",
    error: "Missing or incorrect columns"
  }
  ```

### 2. Calculations and Data Retrieval

#### API 1: Get Highest Volume

**Endpoint**: `/api/highest_volume`  
**Method**: `GET`  
**Parameters**:
- `start_date`: Start date of the range (`YYYY-MM-DD`).
- `end_date`: End date of the range (`YYYY-MM-DD`).
- `symbol` (optional): Filter by stock symbol.

**Description**: Returns the record(s) with the highest volume within the specified date range or for the given symbol.

**Response**:
```json
{
  "highest_volume": {
    "date": "YYYY-MM-DD",
    "symbol": "ULTRACEMCO",
    "volume": 1000000
  }
}
```

#### API 2: Get Average Close Price

**Endpoint**: `/api/average_close`  
**Method**: `GET`  
**Parameters**:
- `start_date`: Start date of the range (`YYYY-MM-DD`).
- `end_date`: End date of the range (`YYYY-MM-DD`).
- `symbol`: Stock symbol to filter.

**Description**: Calculates and returns the average closing price for the specified symbol within the given date range.

**Response**:
```json
{
  "average_close": {
        "symbol": "MUNDRAPORT",
        "average": 456.7691593352886
    }
}
```

#### API 3: Get Average VWAP

**Endpoint**: `/api/average_vwap`  
**Method**: `GET`  
**Parameters**:
- `start_date`: Start date of the range (`YYYY-MM-DD`).
- `end_date`: End date of the range (`YYYY-MM-DD`).
- `symbol` (optional): Filter by stock symbol.

**Description**: Calculates and returns the average VWAP for the specified symbol or within the given date range.

**Response**:
```json
{
  "average_VWAP": {
        "symbol": "MUNDRAPORT",
        "average": 458.52812316715546
    }
}
```

### Query Examples:
- `/api/highest_volume?start_date=2024-01-01&end_date=2024-12-31`
- `/api/average_close?start_date=2024-01-01&end_date=2024-12-31&symbol=ULTRACEMCO`
- `/api/average_vwap?symbol=ULTRACEMCO`

## Database Structure

Each stock entry is stored as a document in the `stock_data` collection with the following structure:

```json
{
  "date": "YYYY-MM-DD",
  "symbol": "ULTRACEMCO",
  "series": "EQ",
  "prev_close": 10.0,
  "open": 305.0,
  "high": 340.0,
  "low": 253.25,
  "last": 259.0,
  "close": 260.0,
  "vwap": 268.8,
  "volume": 6633956,
  "turnover": 1.78E14,
  "trades": 133456,
  "deliverable": 970249,
  "percent_deliverable": 0.1463
}
```

## Testing and Documentation

1. **Postman**: A Postman collection has been created for testing all APIs.
2. **Documentation**: The APIs are documented with descriptions, parameters, and expected responses.
3. **Unit Testing**: Jest is used for unit tests, ensuring validation logic and calculations are correct.

## Postman Collection

A Postman collection is provided with pre-configured requests to test each API. Follow these steps to test the endpoints:
1. Import the Postman collection file.
2. First test the upload csv API by changing the file.
3. Change the parameters accordingly to the file uploaded.
4. Run each API request to check the functionality of the system.

## Unit Tests

- **Validation Logic**: Test to ensure correct validation of CSV data.
- **Calculations**: Test the accuracy of calculations like highest volume, average close, and average VWAP.




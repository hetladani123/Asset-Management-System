-- Create the database


-- Set the search path to the desired schema
CREATE SCHEMA IF NOT EXISTS Hedge_Funds;
SET SEARCH_PATH TO Hedge_Funds;

-- Create the User table
CREATE TABLE Hedge_Funds.User (
    UserId VARCHAR(80) PRIMARY KEY NOT NULL,
    UserName VARCHAR(50) NOT NULL,
    Email VARCHAR(50) NOT NULL,
    PassWord VARCHAR(8) NOT NULL
);

-- Create the Portfolio table
CREATE TABLE Hedge_Funds.Portfolio (
    PortfolioID DECIMAL(9,0) PRIMARY KEY NOT NULL,
    PortfolioName VARCHAR(50) NOT NULL,
    CreationDate DATE NOT NULL,
    UserId VARCHAR(80),
    FOREIGN KEY (UserId) REFERENCES Hedge_Funds.User(UserId)
    ON UPDATE CASCADE ON DELETE SET NULL
);

-- Create the AdminLog table
CREATE TABLE Hedge_Funds.AdminLog (
    UserID VARCHAR(80) NOT NULL,
    ActionType VARCHAR(50) NOT NULL,
    ActionDate TIMESTAMP NOT NULL,
    FOREIGN KEY (UserID) REFERENCES Hedge_Funds.User(UserId)
);

-- Create the Position table
CREATE TABLE Hedge_Funds.Position (
    UserID VARCHAR(80) NOT NULL,
    ActionType VARCHAR(80) NOT NULL,
    ActionDate TIMESTAMP NOT NULL,
    FOREIGN KEY (UserID) REFERENCES Hedge_Funds.User(UserId)
);

-- Create the Asset table
CREATE TABLE Hedge_Funds.Asset (
    AssetID INT PRIMARY KEY,
    AssetName VARCHAR(100) NOT NULL,
    NAV NUMERIC(10,4) NOT NULL
);

-- Create the AssetData table
CREATE TABLE Hedge_Funds.AssetData (
    AssetID INT NOT NULL,
    Date DATE NOT NULL,
    NAV NUMERIC(10,4) NOT NULL,
    FOREIGN KEY (AssetID) REFERENCES Hedge_Funds.Asset(AssetID)
);

-- Create the Transaction table
CREATE TABLE Hedge_Funds.Transaction (
    TransactionID INT PRIMARY KEY NOT NULL,
    Amount INT,
    PortfolioID INT,
    AssetID INT,
    TransactionType VARCHAR(50) NOT NULL,
    Time TIMESTAMP,
    Unit NUMERIC(10,4),
    FOREIGN KEY (PortfolioID) REFERENCES Hedge_Funds.Portfolio(PortfolioID)
        ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY (AssetID) REFERENCES Hedge_Funds.Asset(AssetID)
        ON UPDATE CASCADE ON DELETE CASCADE
);


-- Create the SupportTicket table
CREATE TABLE Hedge_Funds.SupportTicket (
    TicketID INT PRIMARY KEY,
    Status VARCHAR(50) NOT NULL,
    UserId VARCHAR(80),
    CreationTime TIMESTAMP NOT NULL,
    Issue TEXT NOT NULL,
    EndTime TIMESTAMP NOT NULL,
    FOREIGN KEY (UserId) REFERENCES Hedge_Funds.User(UserId)
        ON UPDATE CASCADE ON DELETE CASCADE
);

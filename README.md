-- Step 1: Create the schema
CREATE SCHEMA CW1;
GO

-- Step 2: Create the Trail table
CREATE TABLE CW1.Trail (
    TrailID INT IDENTITY(1,1) PRIMARY KEY,
    TrailName VARCHAR(100) NOT NULL,
    Length DECIMAL(5, 2) NOT NULL CHECK (Length > 0),
    Duration VARCHAR(20) NOT NULL,
    Difficulty VARCHAR(10) CHECK (Difficulty IN ('Easy', 'Moderate', 'Hard')),
    Type VARCHAR(20) CHECK (Type IN ('Walking', 'Cycling', 'Running'))
);
GO

-- Step 3: Create the Location table
CREATE TABLE CW1.Location (
    LocationID INT IDENTITY(1,1) PRIMARY KEY,
    LocationName VARCHAR(100) NOT NULL,
    Latitude DECIMAL(9, 6) NOT NULL CHECK (Latitude BETWEEN -90 AND 90),
    Longitude DECIMAL(9, 6) NOT NULL CHECK (Longitude BETWEEN -180 AND 180)
);
GO

-- Step 4: Create the TrailLocation table
CREATE TABLE CW1.TrailLocation (
    TrailID INT NOT NULL,
    LocationID INT NOT NULL,
    PRIMARY KEY (TrailID, LocationID),
    FOREIGN KEY (TrailID) REFERENCES CW1.Trail(TrailID),
    FOREIGN KEY (LocationID) REFERENCES CW1.Location(LocationID)
);
GO

-- Step 5: Create the Checkpoint table
CREATE TABLE CW1.Checkpoint (
    CheckpointID INT IDENTITY(1,1) PRIMARY KEY,
    TrailID INT NOT NULL,
    CheckpointName VARCHAR(100) NOT NULL,
    CheckpointDescription VARCHAR(255),
    FOREIGN KEY (TrailID) REFERENCES CW1.Trail(TrailID)
);
GO

-- Step 6: Insert sample data
-- Insert data into Trail table
INSERT INTO CW1.Trail (TrailName, Length, Duration, Difficulty, Type) 
VALUES 
    ('Plymouth Waterfront', 3.5, '2 hours', 'Easy', 'Walking'),
    ('Plymbridge Circular', 4.0, '1.5 hours', 'Moderate', 'Cycling');
GO

-- Insert data into Location table
INSERT INTO CW1.Location (LocationName, Latitude, Longitude) 
VALUES 
    ('Start Point', 50.3655, -4.1407),
    ('Plymbridge Woods', 50.3881, -4.0742);
GO

-- Insert data into TrailLocation table
INSERT INTO CW1.TrailLocation (TrailID, LocationID) 
VALUES 
    (1, 1), -- Plymouth Waterfront at Start Point
    (2, 2); -- Plymbridge Circular at Plymbridge Woods
GO

-- Insert data into Checkpoint table
INSERT INTO CW1.Checkpoint (TrailID, CheckpointName, CheckpointDescription) 
VALUES 
    (1, 'Checkpoint 1', 'Scenic viewpoint'),
    (1, 'Checkpoint 2', 'Historical marker'),
    (2, 'Checkpoint 1', 'Bridge view');
GO

-- Step 7: Verify data with SELECT statements
-- Verifying data in Trail table
SELECT * FROM CW1.Trail;

-- Verifying data in Location table
SELECT * FROM CW1.Location;

-- Verifying data in TrailLocation table
SELECT * FROM CW1.TrailLocation;

-- Verifying data in Checkpoint table
SELECT * FROM CW1.Checkpoint;

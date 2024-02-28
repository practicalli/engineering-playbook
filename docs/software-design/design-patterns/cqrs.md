# Command Query Resposibility Segregation

system architecture that extends the idea behind command–query separation to the level of services.[1][2] Such a system will have separate interfaces to send queries and to send commands. As in CQS, fulfilling a query request will only retrieve data and will not modify the state of the system (with some exceptions like logging access), while fulfilling a command request will modify the state of the system.

Many systems push the segregation to the data models used by the system. The models used to process queries are usually called read models and the models used to process commands write models.


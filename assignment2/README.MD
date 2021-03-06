Assignment 2: Replicated KV
=============================

###Overview###

This assignment serves as the basis for the complete implementation of the Raft Consensus Algorithm. The architecture includes a cluster of servers, one amongst them as a leader and multiple clients talking to the leader.

###Mechanism###

* The Client sends a command to the Leader. 
* The Leader broadcasts AppendRPCEntries to all other servers including itself. 
* On receving the AppendRPCEntries, the servers send back an acknowledgement to the Leader. 
* The Leader checks to see if it has a recevied acknowledgements from a majority of servers. If yes, it processes the command on the KV Store and returns appropriate response back to the
 client.
 
###Usage###

####Getting The Code###

* Run the following command:
 
 `go get github.com/pankajrandhe/cs733/assignment2/`
 
####Running####

* Run the following commands:

  `go build github.com/pankajrandhe/cs733/assignment2/raft/`
  
  Please ensure that you have updated the path of the servers_json.json file to your `$GOPATH` in `raft_server.go`, before running the next command.
 
  `go install github.com/pankajrandhe/cs733/assignment2/raft_server/`
 
  `go install github.com/pankajrandhe/cs733/assignment2/runner/`
 
  `runner`
 
  This will start the servers, specified in the servers_json.json file.
  
####Testing####

* Please ensure that servers are running, by starting `runner` in another terminal before testing.

  `go test github.com/pankajrandhe/cs733/assignment2/raft_server/`
 
  
###KV Store Protocol Specification###

* Set: create the key-value pair, or update the value if it already exists.

  `set <key> <exptime> <numbytes> [noreply]\r\n`
  
  `<value bytes>\r\n`

  The server responds with:

  `OK <version>\r\n`

  where version is a unique 64-bit number (in decimal format) assosciated with the key.

* Get: Given a key, retrieve the corresponding key-value pair

  `get <key>\r\n`

  The server responds with the following format (or one of the errors described later)

  `VALUE <numbytes>\r\n`
  
  `<value bytes>\r\n`

* Get Meta: Retrieve value, version number and expiry time left

  `getm <key>\r\n`

  The server responds with the following format (or one of the errors described below)

  `VALUE <version> <exptime> <numbytes>\r\n`
  
  `<value bytes>\r\n`

* Compare and swap. This replaces the old value (corresponding to key) with the new value only if the version is still the same.

  `cas <key> <exptime> <version> <numbytes> [noreply]\r\n`
  
  `<value bytes>\r\n`

  The server responds with the new version if successful (or one of the errors described late)

  `OK <version>\r\n`

* Delete key-value pair

  `delete <key>\r\n`

  Server response (if successful)

  `DELETED\r\n`
  
###Errors Returned###
* `ERRCMDERR` - Commandline Formatting Error
* `ERRNOTFOUND` - Key Not Found Error
* `ERR_VERSION` - Version Mismatch Error
* `ERR_INTERNAL` - Internal Error
* `ERR_REDIRECT host port` - Not a Leader Error

###Group Members###
* Pankaj Randhe - 143310018
* Abhishek Potnis - 143310008
* Mahesh Saini - 133310021




  
   
   
 
 
 

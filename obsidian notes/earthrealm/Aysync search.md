search class
- searchCount 
	- take in a string 
	- output count of number of matches
	- if string is undefined, output ""{ "message": "INVALID_TERM"}
- emmit seearch_started when method is called, and if searchterm is not invalid
- search sucess
	- emitt when ncount maches method resolves seuccefully
	- make an object containing the number of matches and the term
	- pass object out
- search error
	- emmit when searchterm is undefined, or when countmatches fails 
	- make an object containing the error message, and the term used
![[Pasted image 20260518184805.png]]![[Pasted image 20260518184821.png]]
const API = require('./mock-api');

// To count the matches, call API.countMatches(term) where term is the search term

const EventEmitter = require('events');

class Search extends EventEmitter {

  constructor() {

    super()

    this.err = {

      message: "",

      term: ""

    };

    this.result = {

      count: 0,

      term: ""

    };

  }

  

  searchCount(term) {

  

    if (typeof term === 'string') {

  

      this.emit("SEARCH_STARTED", term);

  

      API.countMatches(this.term)

        .then((value) => {

          this.result.count = value;

          this.result.term = term;

          this.emit("SEARCH_SUCCESS", this.result);

  

        })

        .catch((err) => {

          this.err.message = err;

          this.err.term = term;

          this.emit("SEARCH_ERROR", this.err);

  

        })

    }

    else {

      this.err.message = "INVALID_TERM"

      this.err.term = term;

      this.emit("SEARCH_ERROR", this.err);

    }

  }

  

}

module.exports = Search;
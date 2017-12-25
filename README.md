# simple-memcached

A Node.js package that act like a chache system,similarily redis cache and memcached do.
But this one too simple to use.
## Usage

First, install the package using npm:

    npm install simple-memcached --save

Then, require the package and use it like so:

    var simpleCache = require('simple-memcached');

And get data from cache with this simple function call:

    simpleCache.getFromCache("get_all_data");

In this case, get_all_data is the key to get cached data.    
And also set data from database to cache like this:

    simpleCache.setCache("get_all_data", result, 60);

In this case, get_all_data is the key and result is data returned from database. 
60 is the expiry time in seconds for that cache. 

Check this usage sample too;

    exports.get_all_data = function (cb) {
        var get_all_data = simpleCache.getFromCache("get_all_data");
        if(get_all_data !== null){
            cb(null, {status:200,desc:'success from cache', data: get_all_data});
            return;
        }
        var query = "SELECT * FROM table WHERE 1=1";
        db.getConnection(function (err, connection) {
            if(err){
                cb(err);
            }else{
                connection.query(query, function (err, result) {
                    connection.release();
                    if (err) {
                        cb(err);
                    } else {
                        simpleCache.setCache("get_all_data", result, 60);
                        cb(null,{status:200,desc:'success from db', data: result});
                    }
                });
            }
        });
    }

## License

ISC
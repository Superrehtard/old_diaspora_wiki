### Diaspora Comes With Support For Amazon S3 Built In.
You can use Amazon S3 (Simple Storage Service) To Host Many Parts Of Your Pod. This includes the static assets that are created using the assets:precompile rake task built into Diaspora, to do this Diaspora uses the asset-sync gem. You can also store user profile pictures on Amazon S3.

### Storing Static Assets On Amazon S3.

Firstly, cd Into Your Diaspora Code Folder Using The Command Line. Then Run The Following Commands (For A NON Heroku Setup) Replacing 'xxxx' For The Piece Of Info From Your S3 Account:

    export AWS_ACCESS_KEY_ID=xxxx   
    export AWS_SECRET_ACCESS_KEY=xxxx
    export FOG_DIRECTORY=xxxx 
    




Voxdolo was kind enough to refactor our specs to use a [better convention](http://github.com/diaspora/diaspora/commit/81d753e7737d31524adc8984482af87d6dcb9613).
For an example look in /spec/models/album_spec.rb

Here is a summary:

Most time we do not want to redefine a variable for every test so we can set them up using handy let convention. There are two kinds( let & let! ):

     let(:album)     {commands} 

This should be used for the majority of the tests. It sets up a method that lazily evaluates when its called, and once it is evaluated the output can be used for the remaineder of that test. For example:

     let (:user) {Factory.create :user}
     let (:msg) {user.post(:message)}
     let(:album){user.post(:album)}

     it 'says hello world' do 
          msg.message.should == "Hello World"
     end

     it 'album should have a title' do
          album.title.should_not be_nil
     end

when the first test runs it will not post an album
and vice versa

     let!(:something) {commands} <--- evaluates right then and there

You want to use the let! when you want the function to be evaluated before the test. For example, in /spec/models/visible_posts_spec.rb we want status messages to be already in the database so that we can query for them:

     let!(:user) {Factory.create :user}
     let!(:status_message) {user.post(:status_message, :message => "hi", :to => user2.aspects.first.id)}

     it 'queries by person id' do
          posts = user2.visible_posts(:person_id => user2.person.id)
          posts.should == [status_message]
     end



There are also several kinds of blocks:

     before do              <-- a block that runs before each test in the given section
          command1
          ....
     end


     describe '#next_photo' do        <---- is a convention of grouping tests on individual methods.
          it 'tests something' do
               ....
          end
     end

     context 'when an album has two attached images' do         <--- is a convention to group test with a certain setup.
     
     end
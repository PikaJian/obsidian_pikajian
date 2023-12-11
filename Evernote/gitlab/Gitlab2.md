# StackEdit

## How to reset your root password

Log into your server with root privileges. Then start a Ruby on Rails console.
Start the console with this command:

    gitlab-rails console production
    

Wait until the console has loaded.
There are multiple ways to find your user. You can search for email or username.

    user = User.where(id: 1).first
    

or

    user = User.find_by(email: 'admin@local.host')
    

Now you can change your password:

    user.password = 'secret_pass'
    user.password_confirmation = 'secret_pass'
    

It’s important that you change both password and password\_confirmation to make it work.
Don’t forget to save the changes.

    user.save!
    

Exit the console and try to login with your new password.

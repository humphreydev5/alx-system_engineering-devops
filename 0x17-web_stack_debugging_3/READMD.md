![web pack](https://github.com/humphreydev5/alx-system_engineering-devops/assets/134397026/be646421-c1d8-4108-8bc2-86e203091ccd)

# 0x17. Web stack debugging #3


When debugging, sometimes logs are not enough. Either because the software is breaking in a way that was not expected and the error is not being logged, or because logs are not providing enough information. In this case, you will need to go down the stack, the good news is that this is something Holberton students can do :)

WordPress is a very popular tool, it allows you to run blogs, portfolios, e-commerce, and company websites... It powers 26% of the web, so there is a fair chance that you will end up working with it at some point in your career.

WordPress is usually run on LAMP (Linux, Apache, MySQL, and PHP), which is a very widely used set of tools.

The web stack you are debugging today is a WordPress website running on a LAMP stack.

0. Strace is your friend

Using strace, find out why Apache is returning a 500 error. Once you find the issue, fix it and then automate it using Puppet (instead of using Bash as you were previously doing).

Hint:

strace can attach to a current running process
You can use tmux to run strace in one window and curl in another one
Requirements:

Your 0-strace_is_your_friend.pp file must contain Puppet code
You can use whatever Puppet resource type you want for you fix


# Requirements
## General
All your files will be interpreted on Ubuntu 14.04 LTS
All your files should end with a new line
A README.md file at the root of the folder of the project is mandatory
Your Puppet manifests must pass puppet-lint version 2.1.1 without any errors
Your Puppet manifests must run without error
Your Puppet manifests first line must be a comment explaining what the Puppet manifest is about
Your Puppet manifests files must end with the extension .pp
Files will be checked with Puppet v3.4

> [!TIP]
$ apt-get install -y ruby
$ gem install puppet-lint -v 2.1.1

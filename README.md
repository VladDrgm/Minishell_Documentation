Tests to be performed while evaluating the Minishell / while building up Minishell:


ENV VARIABLES:
  - when launching bash (from any application), the 'env' command will print the environmental variables
  - if a student unsets the variables BEFORE launching Minishell, the Minishell program must be able to replicate bash behaviour, according to the subject, as such:
      1) unset PATH
      2) launch minishell
      3) 'echo $PATH' should result in the PATH variable being printed
      4) 'env' should result in the environmental list to be printed, including the PATH variable
  - same for 'PWD' -> if someone unsets PWD, this variable must be recreated when launching Minishell and included in the env list
      1) for PWD only, please note that if you are positioned in a symlink when opening minishell, the 'echo $PWD' command must deliver the correct path you are on.
        STEPS to replicate this behavior:
        a) in the minishell directory: 'mkdir testdir'
        b) 'ln -s testdir link'
        c) move into the link: cd link
        d) create another directory: mkdir testdir2
        e) create another link: ln -s testdir2 link2
        f) now, if you write 'pwd', you should be positioned into your first link. The result should be something like: "~>/..../.../minishell/link"
        g) now launch minishell from this position, as such: "../minishell"
        h) now write "pwd" -> the result should be the same as the one you had previously, before launching minishell: "~>/..../.../minishell/link"
        i) check $PWD -> 'echo $PWD', should be the same as the command 'pwd'
        i) move into the second link: cd link2
        j) 'pwd' should result in:  "~>/..../.../minishell/link/link2"
        k) 'echo $PWD' should also result in "~>/..../.../minishell/link/link2"
        l) unset PWD
        m) 'echo $PWD' shoudl result in an empty newline
        n) 'pwd' should still print the path: "~>/..../.../minishell/link/link2"
        o) cd ..
        p) 'env' should present a list WITHOUT PWD in it
        r) 'echo $PWD' should print "~>/..../.../minishell/link"
        s) 'pwd' should also print "~>/..../.../minishell/link"
        t) 'echo $OLDPWD' should result in an empty space

BUILTINS:
*echo:
  - echo lala => lala\n
  - echo $PWD => should print the value of the PWD env variable
  - echo -n lol => lol
  - echo -n -nnnnn -nnnnnnnn => (no empty lines; nothing to be printed)
  - echo -nnnnnn -nnnnnn -nnnnnk => -nnnnnk
  - echo-nnnnnn => errror
*pwd
  - prints the current folder one is in, no matter if $PATH exists or not
  - must correctly print symlinks, if within a symlink structure, as such: :~>/..../.../minishell/symlink1/symlink2/testdir3/symlink4
*export
  -'export ABC=lala' must add to the env list the variable ABC with the value lala
  -'export A*$#$#BC=lala' must throw an error
  -'export' must print a list with all the env variables, displayed in alphabetical order, as such:
          declare -x USER="username"
  -'export ABC=lala ADF=testing AJH=oops' should add all 3 variables to the env
  -'export ABC=lala' and 'export ABC=lala' will export the ABC variable only once, with the value 'lala'
  -'export ABC=lala' and 'export ABC=lolo' will export the ABC variable only once, but the second command will change its value to 'lolo'

*unset
  - 'unset ABC' will unset the variable ABC, removing it from the env variables
  - 'unset ABC ABD ABF' will unset all 3 variables from env, if they are present in the env variables

*cd
  - 'cd /' should change directory to '/'
  - 'cd /home/user/Documents' should change the directory to '/home/user/Documents'
  - 'cd /home/user/Documents/symlink and then write pwd' -> the display should be /home/user/Documents/symlink and not /home/user/Documents/*directory towards which the link points to*
  - 'cd $OLDPWD' should change directory to the one indicated by OLDPWD
  - 'cd ..' should change the directory to the one above

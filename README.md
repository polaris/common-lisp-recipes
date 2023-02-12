# Common Lisp Recipes

## Creating a project skeleton

```
* (ql:quickload "quickproject")
* (quickproject:make-project #p"~/src/myproject/" :depends-on '(drakma cxml))
* (asdf:load-system "myproject")
```

## Building self-contained executables

### SBCL

#### `SB-EXT:SAVE-LISP-AND-DIE`

https://koji-kojiro.github.io/sb-docs/build/html/sb-ext/function/SAVE-LISP-AND-DIE.html

##### Example
```
sbcl --eval '(ql:quickload :myproject)'                                            \
     --eval "(sb-ext:save-lisp-and-die #p\"myproject\" :compression 9              \
                                                       :toplevel #'myproject::main \
                                                       :executable t)"
```

### buildapp

https://www.xach.com/lisp/buildapp/

#### Usage

##### Example 1
```
buildapp --output myproject                            \
         --compress-core                               \
         --asdf-path ~/src/myproject/                  \
         --load-system myproject                       \
         --eval '(defun main (args) (myproject:main))' \
         --entry main
```

##### Example 2
```
buildapp --eval '(defun main (argv) (declare (ignore argv)) (write-line "Hello, world"))' \
         --entry main                                                                     \
         --compress-core                                                                  \
         --output hello-world
```

## Executing a shell command

```
(ql:quickload "inferior-shell")
(inferior-shell:run "ls"
                    :on-error nil
                    :error-output :string
                    :output :string)
```

## Scheduling a job

```
(ql:quickload "clerk")
(clerk:job "print hello" every 5.seconds (print "hello"))
(clerk:start)
...
(clerk:stop)
(clerk:empty-jobs-queue)
```

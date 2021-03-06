Author: MartinRamm

This is something I created to increase my productivity with docker/docker-compose.
Feel free to create a PR with improvements - but please keep this documentation up to date!

# Requirements

1. `docker` / `docker-compose` must be accessible to non-root users
1. `fzf`  (https://github.com/junegunn/fzf#installation)

# Installation instructions

1. Clone this repository: `git clone https://github.com/MartinRamm/fzf-docker.git`
1. Add to your `.zshrc` or `.bashrc` file this command: `source /path/to/docker-fzf`
1. (Optional): Customize the `de` command as described in _[Default command for `de`](#default-command-for-de)_

# Overview of available commands

| command | description                                                                        | fzf mode | command arguments (optional)                                                                                 |
| ------- | ---------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------ |
| dr      | docker restart && open logs (in follow mode)                                       | multiple |                                                                                                              |
| dl      | docker logs (in follow mode)                                                       | multiple | time interval - e.g.: `1m` for 1 minute - (defaults to all available logs)                                   |
| dla     | docker logs (in follow mode) all containers                                        |          | time interval - e.g.: `1m` for 1 minute - (defaults to all available logs)                                   |
| de      | docker exec in interactive mode                                                    | single   | command to exec (default - see below)                                                                        |
| drm     | docker remove container (with force)                                               | multiple |                                                                                                              |
| drma    | docker remove all containers (with force)                                          |          |                                                                                                              |
| ds      | docker stop                                                                        | multiple |                                                                                                              |
| dsa     | docker stop all running containers                                                 |          |                                                                                                              |
| dsrm    | docker stop and remove container                                                   | multiple |                                                                                                              |
| dsrma   | docker stop and remove all container                                               |          |                                                                                                              
| dk      | docker kill                                                                        | multiple |                                                                                                              |
| dka     | docker kill all containers                                                         |          |                                                                                                              |
| dkrm    | docker kill and remove container                                                   | multiple |                                                                                                              |
| dkrma   | docker kill and remove all container                                               |          |                                                                                                              |
| drmi    | docker remove image (with force). This includes options to remove dangling images. | multiple |                                                                                                              |
| drmia   | docker remove all images (with force). This includes dangling images.              |          |                                                                                                              |
| dclean  | `dsrma` and `drmia`                                                                |          |                                                                                                              |
| dcu     | docker-compose up (in detached mode)                                               | multiple | path to docker-compose file (defaults to recursive search for `docker-compose.yml` or `docker-compose.yaml`) |
| dcua    | docker-compose up all services (in detached mode)                                  |          | path to docker-compose file (defaults to recursive search for `docker-compose.yml` or `docker-compose.yaml`) |
| dcb     | docker-compose build (with --no-cache and --pull)                                  | multiple | path to docker-compose file (defaults to recursive search for `docker-compose.yml` or `docker-compose.yaml`) |
| dcba    | docker-compose build (with --no-cache and --pull) all                              |          | path to docker-compose file (defaults to recursive search for `docker-compose.yml` or `docker-compose.yaml`) |
| dcp     | docker-compose pull                                                                | multiple | path to docker-compose file (defaults to recursive search for `docker-compose.yml` or `docker-compose.yaml`) |
| dcpa    | docker-compose pull all services                                                   |          | path to docker-compose file (defaults to recursive search for `docker-compose.yml` or `docker-compose.yaml`) |
| dcupd   | docker-compose update image (rebuild or pull)                                      | multiple | path to docker-compose file (defaults to recursive search for `docker-compose.yml` or `docker-compose.yaml`) |
| dcupda  | `dcba` and `dcpa`                                                                  |          | path to docker-compose file (defaults to recursive search for `docker-compose.yml` or `docker-compose.yaml`) |

## Default command for `de`
The command used to `exec` into a container is dependent on the base image.
The fallback command used to `exec` into a container is similar to `zsh || bash || ash || sh`.
Useful standards are already implemented for images like `mysql` or `mongo` (PRs to add more default commands are appreciated).

You may however add custom commands that `de` will then use to `exec` into a container. To do this
1. `cd /path/to/docker-fuzzy-search-commands`
1. copy the `.docker-fuzzy-search-exec.template` to your home directory, omitting the `.template` extension: 
`cp {,~/}.docker-fuzzy-search-exec.template && mv ~/.docker-fuzzy-search-exec{.template,}`
1. Customize the script as described in the file.

# Learning by doing
### fzf mode = single
The image below shows a user `exec`ing into the container `infrastructure_some-mysql_1_6fe4edd94d07` container with the `de` command.
Because this script has a sensible default command registered for the base image of this container, `mysql` is directly opened (with the password set in the environment variable).

The command `de` was entered into a terminal. The user now typed `sql` to narrow the search for containers that contain that phrase. When the correct container was selected by the user pressing `Enter`.
Alternatively, the user could have used the arrow keys to select the correct container name.

![example gif](single.gif)

### optional command arguments
This image shows the `dl` command executed with `10m` as an optional argument. 
Therefore the command will only show the logs of the selected container produced in the last 10 minutes, instead of all available logs for this container.

![example gif](args.gif)

### fzf mode = multiple

The image below shows a user starting the services `whiteboard-1.0` and `redis` with the `dcu` command.
To mark a containers/services in fzf, press on the `tab` key. To deselect, press `shift + tab`.
To remove the input, press `Alt + Backspace`.
Finally, press `Enter` to start the command. Note that when pressing `Enter`, the selected item will *not* be added automatically to your selection.
If you only want to mark one container, you don't have to select it with tab - you can follow the instructions of fzf single mode.

![example gif](multiple.gif)

# License
This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

In jurisdictions that recognize copyright laws, the author or authors
of this software dedicate any and all copyright interest in the
software to the public domain. We make this dedication for the benefit
of the public at large and to the detriment of our heirs and
successors. We intend this dedication to be an overt act of
relinquishment in perpetuity of all present and future rights to this
software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <http://unlicense.org>

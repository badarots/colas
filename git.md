## Revertendo commits:

Lots of complicated and dangerous answers here, but it's actually easy:

    git revert --no-commit 0766c053..HEAD
    git commit

This will revert everything from the HEAD back to the commit hash, meaning it will recreate that commit state in the working tree as if every commit since had been walked back. You can then commit the current tree, and it will create a brand new commit essentially equivalent to the commit you "reverted" to.

(The --no-commit flag lets git revert all the commits at once- otherwise you'll be prompted for a message for each commit in the range, littering your history with unnecessary new commits.)

This is a safe and easy way to rollback to a previous state. No history is destroyed, so it can be used for commits that have already been made public.

## Copiar branch remoto

    git checkout <branch>

## Submodulos

Para adcioná-los:

	git submodule add https://github.com/<user>/<repo> <repo>

Para carregar os arquivos:

    git submodule update --init --recursive

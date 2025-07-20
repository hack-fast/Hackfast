### **PULL REQUESTS**

You can contribute to HackFast by making a [pull request] that will be reviewed by maintainers and integrated into the main repository when the changes made are approved. You can contribute changes to the documentation, Expand an existing section or add a new technique, tool or cheat-sheet.

[pull request]: https://docs.github.com/en/pull-requests

### **LEARNING ABOUT PULL REQUESTS**

Pull requests are a concept layered on top of Git by services that provide Git hosting. Before you consider making a pull request, you should familiarize yourself with the documentation on GitHub, the service we are using. The
following articles are of particular importance:

1. [Forking a repository]
2. [Creating a pull request from a fork]
3. [Creating a pull request]

Note that they provide tailored documentation for different operating systems and different ways of interacting with GitHub. We do our best in the documentation here to describe the process as it applies to HackFast but cannot cover all possible combinations of tools and ways of doing things. It is also important that you understand the concept of a pull-request in general before continuing.

[Forking a repository]: https://docs.github.com/en/get-started/quickstart/fork-a-repo
[Creating a pull request from a fork]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork
[Creating a pull request]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request

### **PULL REQUEST PROCESS**

In the following, we describe the general process for making pull requests. 

### **PREPARING CHANGES AND DRAFT PR**

The diagram below describes what typically happens to repositories in the process or preparing a pull request. We will be discussing the review-revise process below. It is important that you understand the overall process first
before you worry about specific commands. This is why we cover this first before providing instructions below.

``` mermaid
sequenceDiagram
  autonumber

  participant HackFast
  participant PR
  participant fork
  participant local

  HackFast ->> fork: fork on GitHub
  fork ->> local: clone to local
  local ->> local: branch
  loop prepare
    loop push
      loop edit
        local ->> local: commit
      end
      local ->> fork: push
    end
    HackFast ->> fork: merge in any changes
    fork ->>+ PR: create draft PR
    PR ->> PR: review your changes
  end
```

1. The first step is that you create a fork of the Hackfast repository [HackFast]. This provides you with a repository that you can push changes to. Note that it is not possible to have more than one fork of a given repository at any point in time. So, the fork you create will be *the* fork you have.

2. Once it is made, clone it to your local machine so you can start working on your changes.

3. All contributions should be made through a 'topic branch' with a name that describes the work being done. This allows you to have more than one piece of work in progress and, The topic branch will be relatively short-lived and will disappear at the end, when your changes have been incorporated into the documentation.

4.  Next comes the iterative process of making edits, committing them to your clone. Please commit in sensible chunks that constitute a piece of work instead of committing everything in one go. Remember that fine-grained, incremental commits are much easier to review in than large changes all over the place and with many files involved. Try to keep your changes as small and localized as possible and keep the reviewer in mind when committing. In particular, make sure to write meaningful commit messages.

6. Push your work up to your fork regularly.

7. You should also keep an eye on changes in the HackFast repository you cloned. This is especially important if you work takes a while. Please try and merge any concurrent changes into your fork and into your branch regularly. You *must* do this at least once before creating a pull request, so make your life easier and do it more often so as to minimize the risk of conflicting changes.

8. Once you are happy that your changes are in a state that you can describe them in a *draft* pull request, you should create this. Make sure to reference any previous discussions or issues that gave rise to your work. Creating a draft is a good way to get *early* feedback on your work from the maintainer or others. You can explicitly request reviews at points where you think this would be important.

9.  Review your work as if you were the reviewer and fix any issues with your work so far.

[HackFast]: https://github.com/hack-fast/HackFast.git


### **FINALIZING**

Once you are happy with your changes, you can move to the next step, finalizing your pull request and asking for a more formal and detailed review. The diagram below shows the process:

``` mermaid
sequenceDiagram
  autonumber
  participant HackFast
  participant PR
  participant fork
  participant local

  activate PR
  PR ->> PR : finalize PR
  loop review
    loop discuss
      PR ->> PR: request review
      PR ->> PR: discussion
      local ->> fork: push further changes
    end
    PR ->> HackFast: merge (and squash)
    deactivate PR
    fork ->> fork: delete branch
    HackFast ->> fork: pull
    local ->> local: delete branch
    fork ->> local: pull
  end
```

1. When you are happy that the changes you made amount to a contribution that the maintainer(s) could integrate into the main repository, finalize the pull request. This signals to everyone that consider the work 'done' and that it can be reviewed with a view to accepting and integrating it.

3.  The maintainer may make comments, which you should discuss with them. Bear in mind when doing this that the maintainer may have a different point of view compared to yours. Please keep the discussion respectful at all times. 

4. Make any requested changes by committing them to your local clone and pushing them up to your fork. This will automatically update the pull request. It may well take a few iterations to get your contributions to an acceptable
state. You can help the process along by carefully reading comments made and making changes with care.

5. Once the reviewer is fully satisfied with the changes, they can merge them into the main branch (or 'master'). In the process, they may 'squash' your commits together into a smaller number of commits and may edit the messages
that describe them. Congratulations, you have now contributed to this project and should see the changes in the main branch under your name.

6. You can now delete the fork and your local repository and start afresh again next time around. Alternatively, you can keep the repository and local clone around but it is important that you keep them in sync with the upstream
repository for any subsequent work. We recommend that you start by deleting the branch you used on your fork.

7. To make sure you have the changes you produced, pull them from the main repository into the main branch of your fork.

8. Similarly, delete the topic branch from your local clone and...

9. pull the changes to its master branch.

### **MERGING CONCURRENT CHANGES**

If the work you do takes some time then the chances increase that changes will be made to the main repository while you work. It is probably a good idea to set up the original HackFast repository as an `upstream` repository for your local clone.

This is what it might look like:

```bash hl_lines="4"
$ git remote -v
origin	git@github.com:<your_username>/HackFast.git (fetch)
origin	git@github.com:<your_username>/HackFast.git (push)
$ git remote add upstream https://github.com/hack-fast/HackFast.git
$ git remote -v
origin	git@github.com:rikozi/HackFast.git (fetch)
origin	git@github.com:rikozi/HackFast.git (push)
upstream	https://github.com/hack-fast/HackFast.git (fetch)
upstream	https://github.com/hack-fast/HackFast.git (push)
```

After you have done this, you can pull any concurrent changes from the upstream repository directly into your clone and do any necessary merges there, then push them up to your fork. You will need to be explicit about which remote repository you want to use when you are doing a `pull`:

```bash
# making and committing some local changes
push pull upstream master
```

This fetches changes from the `master` branch into your topic branch and merges them.
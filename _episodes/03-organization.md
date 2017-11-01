---
title: "Creating an OMERO server"
teaching: 10
exercises: 0
objectives:
- "Experience the installation of an OMERO server via Ansible"
keypoints:
- "Vagrantfile defines your VM"
- "OME publish example playbooks"
---
OME have published (and maintain) a set of example playbooks.

Here we're going to execute one of those examples.

> ## Don't worry!
>
> Most of the following commands include some BASH substitutions to simplify connecting to a Vagrant VM from different machines. It's really not as complicated as this makes it look.
>
{: .callout}

1 Acquire a copy of the examples (you may have done this already):

~~~
Git clone https://github.com/ome/ansible-examples-omero
~~~
{: .bash} 

Change directory to the clone of ansible-examples-omero
~
cd ansible-examples-omero/public 
~~~
{: .bash} 

Update the vagrantfile (possibly do a manual edit?)
~~~
rm Vagrantfile && wget https://gist.githubusercontent.com/kennethgillen/648105ba0f78440ca41e45963c471744/raw/c6e05535bb20ce08e515d0a10615406838728291/Vagrantfile
~~~
{: .bash} 

To Explain
(Takes 30s..)
~~~
vagrant up
~~~
{: .bash} 


Create a hosts file:
~~~
echo "localhost ansible_port=$(vagrant ssh-config | grep Port | awk '{print $2}') ansible_user=vagrant " > hosts-file
~~~
{: .bash}

It should look similar to:
~~~
cat hosts-file
~~~
{: .bash}
~~~
localhost ansible_port=2222 ansible_user=vagrant
~~~
{: .output}


> ## Asking Vagrant how to connect to the virtual machine
> 
> Vagrant takes care of running >1 machine at once by 
> assigning the VMs different port numbers for SSH for example.
> We can ask vagrant the SSH port of the current machine, which we
> need to give to the ansible-playbook command, via the host file.
> See [docs.ansible.com](http://docs.ansible.com/ansible/latest/intro_inventory.html) 
> for more. These shell snippets acheive the function of asking vagrant for these details.
>
<br/>
>What port is the current Vagrant VM using for SSH?
>~~~
>echo $(vagrant ssh-config | grep Port | awk '{print $2}')
>~~~
>{: .bash} 
>
>
<br/>
>Where is the SSH key used to connect to the current Vagrant VM's `vagrant` user?
>~~~
>echo ${vagrant ssh-config | grep IdentityFile | awk '{print $2}'}
>~~~
>{: .bash} 
{: .solution}

Verify it's working:
~~~
ansible -i hosts-file -m ping localhost --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}')
~~~
{: .bash}

>## Seeing ssh errors?
>
>If you've an `~/.ssh/known_hosts` entry for `localhost` then this new 
>VM is likely to conflict. You will have to delete the existing entry 
>with the corresponding line number in the output from Ansible.
>This can be done manually, or with the following snippets:
>~~~
># Looking in the output for known_hosts:line-number
>echo $(ansible -i hosts-file -m ping localhost --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}')  | grep -oP '(?<=hosts:)\d+')
># Use sed to delete that line. Replace "LINENUMBER" with the line number from the output above.
>sed -i.bak -e "LINENUMBERd" ~/.ssh/known_hosts
>~~~
>{: .bash} 
> Next time you run the `ansible -m ping` command, you'll be asked whether you trust the host key is correct. Type `yes` to continue.
{: .solution}

~~~
ansible-playbook -i hosts-file --private-key  $(vagrant ssh-config | grep IdentityFile | awk '{print $2}') --become  playbook.yml
~~~
{: .bash}

The README.md explains what we need to do:

Each lesson is made up of *episodes* that are 10-30 minutes long
(including time for both teaching and exercises).
The episodes of this lesson explain the tools we use to create lessons
and the formatting rules those lessons must follow.

> ## Why "Episodes"?
>
> We call the parts of lessons "episodes" because
> every other term (like "topic") already has multiple meanings,
> and because it encourages us to think of breaking up our lessons
> into chunks that are about as long as a typical movie scene,
> which is better for learning than long blocks without interruption.
{: .callout}

Our lessons need artwork,
CSS style files,
and a few bits of Javascript.
We could load these from the web,
but that would make offline authoring difficult.
Instead, each lesson's repository is self-contained.

The diagram below shows how source files and directories are laid out,
and how they are mapped to destination files and directories:

![Source and Destination Files]({{ page.root }}/fig/file-mapping.svg)

> ## Collections
>
> As described [earlier]({{ page.root }}/02-tooling/#collections),
> files that appear as top-level items in the navigation menu are stored in the root directory.
> Files that appear under the "extras" menu are stored in the `_extras` directory,
> while lesson episodes are stored in the `_episodes` directory.
{: .callout}

## Helper Files

As is standard with [Jekyll][jekyll] sites,
page layouts are stored in `_layouts`
and snippets of HTML included by these layouts are stored in `_includes`.
Each of these files includes a comment explaining its purpose.

Authors do not have to specify that episodes use the `episode.html` layout,
since that is set by the configuration file.
Pages that authors create should have the `page` layout unless specified otherwise below.

The `assets` directory contains the CSS, Javascript, fonts, and image files
used in the generated website.
Authors should not modify these.

# Standard Files

When the lesson repository is first created,
the initial author should create a `README.md` file containing
a one-line explanation of the lesson's purpose.

The [lesson template]({{ site.template_repo }}) provides the following files
which should *not* be modified:

*   `CONDUCT.md`: the code of conduct.
*   `LICENSE.md`: the lesson license.
*   `Makefile`: commands for previewing the site, cleaning up junk, etc.

## Starter Files

The `bin/lesson_initialize.py` script creates files that need to be customized for each lesson:

`CONTRIBUTING.md`
:   Contribution guidelines.
    The `issues` and `repo` links at the bottom of the file must be changed
    to match the URLs of the lesson:
    look for uses of `FIXME`.

`_config.yml`
:   The [Jekyll][jekyll] configuration file.
    This must be edited so that its links and other settings are correct for this lesson.
    *   `carpentry` should be either "dc" (for Data Carpentry) or "swc" (for Software Carpentry).
    *   `title` is the title of your lesson,
        e.g.,
        "Defence Against the Dark Arts".
    *   `email` is the contact email address for the lesson.

`CITATION`
:   A plain text file explaining how to cite this lesson.

`AUTHORS`
:   A plain text file listing the names of the lesson's authors.

`index.md`
:   The home page for the lesson.
    1.  It must use the `lesson` layout.
    2.  It must *not* have a `title` field in its [YAML][yaml] header.
    3.  It must open with a few paragraphs of explanatory text.
    4.  That introduction must be followed by a single `.prereq` blockquote
        detailing the lesson's prerequisites.
        (Setup instructions appear separately.)
    5.  That must be followed by inclusion of `syllabus.html`,
        which generates the syllabus for the lesson
        from the metadata in its episodes.

`reference.md`
:   A reference guide for the lesson.
    The template will automatically generate a summary of the episodes' key points.
    1.  It must use the `reference` layout.
    2.  Its title must be `"Reference"`.
    3.  Its permalink must be `/reference/`.
    4.  It should include a glossary, laid out as a description list.
    5.  It may include other material as appropriate.

`setup.md`
:   Detailed setup instructions for the lesson.
    Note that we usually divide setup instructions by platform,
    e.g.,
    include level-2 headings for Windows, Mac OS X, and Linux
    with instructions for each.
    The [workshop template]({{ site.workshop_repo }})
    links to the setup instructions for core lessons.
    1.  It must use the `page` layout.
    2.  Its title must be `"Setup"`.
    3.  Its permalink must be `/setup/`.
    4.  It should include whatever setup instructions are required.

`_extras/about.md`
:   General notes about this lesson.
    This page includes brief descriptions of Software Carpentry and Data Carpentry,
    and is a good place to put institutional acknowledgments.

`_extras/discussion.md`
:   General discussion of the lesson contents for learners who wish to know more:
    This page normally includes links to further reading
    and/or brief discussion of more advanced topics.
    1.  It must use the `page` layout.
    2.  Its title must be `"Discussion"`.
    3.  Its permalink must be `/discuss/`.
    4.  It may include whatever content the author thinks appropriate.

`_extra/figures.md` and `_includes/all_figures.html`
:   Does nothing but include `_includes/all_figures.html`,
    which is (re)generated by `make lesson-figures`.
    This page displays all the images referenced by all of the episodes,
    in order,
    so that instructors can scroll through them while teaching.

`_extras/guide.md`
:   The instructors' guide for the lesson.
    This page records tips and warnings from people who have taught the lesson.
    1.  It must use the `page` layout.
    2.  Its title must be `"Instructors' Guide"`.
    3.  Its permalink must be `/guide/`.
    4.  It may include whatever content the author thinks appropriate.

{% include links.md %}

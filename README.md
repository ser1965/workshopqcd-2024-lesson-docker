# Lesson: Dataset scouting

## Building the lesson

This lesson was built from The Carpentries Workbench template lesson following the [quick-start instructions](https://carpentries.github.io/sandpaper-docs/introduction.html#super-quickstart-copy-a-template-from-github). The repository was created checking the option of including all branches and making it public. 

The content can be edited in the `main` branch and the GitHub actions generate and deploy the site to https://cms-opendata-workshop.github.io/workshopqcd-2024-lesson-docker/index.html
No attempt for a local build was made, and in principle, there is no need to install R or Pandoc. Note, however, that the content should be in [Pandoc-flavored Markdown](https://pandoc.org/MANUAL.html). If one wishes to test locally, the instructions are provided in [The Carpentries Workbench][workbench] documentation.

The setup instructions are in `learners/setup.md` and the separate pages under `episodes`.

The `md-outputs` branch shows after setting up the repository. No need to merge them.

The schedule shows only in the "Instructor view": https://cms-opendata-workshop.github.io/workshopqcd-2024-lesson-docker/instructor/index.html 
Use this link if you want to show it.

## Updating an old lesson to the new template

There might be a tool somewhere, but if doing it by hand:

1. Change questions (note the empty line after items), objectives and keypoints to
   ```
   :::::: questions
   - question 1
   - question 2

   ::::::

   :::::: objectives
   - objective 1
   - objective 2

   ::::::

   <!-- EPISODE CONTENT HERE -->

   :::::: keypoints
   - keypoint 1
   - keypoint 2
   ::::::
   ```
2. Remove the double quotes of the question, objectives and keypoints.
3. Make sure that keypoints are at the end of the text.
4. Find all `{: .callout}`, `{: .challenge}`, `{: .testimonial}` etc tags and and remove the preceeding `> ` for the block and change them to

   ```
   ::: callout
   This is a callout block. It contains at least three colons
   :::
   ```
   or

   ```
   ::::::::::::::::::::::::::::::::::::: challenge

   ## Question

   Q: question

   :::::::::::::::: solution

   A: answer

   :::::::::::::::::::::::::
   :::::::::::::::::::::::::::::::::::::::::::::::
   ```

## Notes

### Documentation

See e.g.

- https://carpentries.github.io/lesson-development-training/
- https://carpentries.github.io/sandpaper-docs/index.html


### Indents and unexpected code blocks

Note that double-indent (two tabs) or anything more that three spaces produces a code block. That's not necessarily what one would expect.
Also, in some special cases, in nested lists, the second level items might appear as a code block.
 
### Figures

Figures should be located under `episodes/fig`, and included, for example, with

```
![](fig/portal_screenshot_landing_page.png)
```


## Configuring the lesson

Follow the steps below to
complete the initial configuration of a new lesson repository:

1. **Make sure GitHub Pages is activated:**
   navigate to _Settings_,
   select _Pages_ from the left sidebar,
   and make sure that `gh-pages` is selected as the branch to build from.
   If no `gh-pages` branch is available, check _Actions_ to see if the first
   website build workflows are still running.
   The branch should become available when those have completed.

   All this happened automatically for this repository. 
1. **Adjust the `config.yaml` file:**
   this file contains global parameters for your lesson site.
   Individual fields within the file are documented with comments (beginning with `#`)
   At minimum, you should adjust all the fields marked 'FIXME':
   - `title`
   - `created`
   - `keywords`
   - `life_cycle` (the default, _pre-alpha_, is the appropriate for brand new lessons)
   - `contact`
1. **Annotate the repository** with site URL and topic tags:
   navigate back to the repository landing page and
   click on the gear wheel/cog icon (similar to ⚙️) 
   at the top-right of the _About_ box.
   Check the "Use your GitHub Pages website" option,
   and [add some keywords and other annotations to describe your lesson](https://cdh.carpentries.org/the-carpentries-incubator.html#topic-tags)
   in the _Topics_ field.
   At minimum, these should include:
   - `lesson`
   - the life cycle of the lesson (e.g. `pre-alpha`)
   - the human language the lesson is written in (e.g. `deutsch`)
1. **Adjust the 
   `CITATION.cff`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, and `LICENSE.md` files**
   as appropriate for your project.
   -  `CITATION.cff`:
      this file contains information that people can use to cite your lesson,
      for example if they publish their own work based on it.
      You should [update the CFF][cff-sandpaper-docs] now to include information about your lesson,
      and remember to return to it periodicallt, keeping it updated as your
      author list grows and other details become available or need to change.
      The [Citation File Format home page][cff-home] gives more information about the format,
      and the [`cffinit` webtool][cffinit] can be used to create new and update existing CFF files.
   -  `CODE_OF_CONDUCT.md`: 
      if you are using this template for a project outside The Carpentries,
      you should adjust this file to describe 
      who should be contacted with Code of Conduct reports,
      and how those reports will be handled.
   -  `CONTRIBUTING.md`:
      depending on the current state and maturity of your project,
      the contents of the template Contributing Guide may not be appropriate.
      You should adjust the file to help guide contributors on how best
      to get involved and make an impact on your lesson.
   -  `LICENSE.md`:
      in line with the terms of the CC-BY license,
      you should ensure that the copyright information 
      provided in the license file is accurate for your project.

[cff-home]: https://citation-file-format.github.io/
[cff-sandpaper-docs]:  https://carpentries.github.io/sandpaper-docs/editing.html#making-your-lesson-citable
[cffinit]: https://citation-file-format.github.io/cff-initializer-javascript/
[workbench]: https://carpentries.github.io/sandpaper-docs/

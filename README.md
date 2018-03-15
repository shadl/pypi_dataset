PyPI dataset
============

This dataset was created to estimate impact of network effects on
project sustainability in Open Source.
We used Python Package Index (PyPI) as an example of a package 
ecosystem to study.

The data in this dataset represent PyPI state and package 
repositories as of early Jan 2018.

Please see the publication 
(link will be posted once it is accepted)
for more details on how this dataset was created and its 
limitations.

The data is stored as CSV files compressed into `data.zip`, 
due to GitHub limitations on the size of published content. 

Data description
---------------- 

**raw_dependencies.csv**
contains all package releases and their respective dependencies.
Each line represents a package release.
 
columns:
- *name*: package name
- *version*: package version
- *date*: the date of the earliest release files, YYYY-MM-DD. 
    Some releases contain several downloadable files for 
    different platforms, e.g. Windows, Linux, 32 or 64 bits.
    We use earliest of these date as the release date.
- *deps*: comma-separated list of package names this package is
    dependent upon, excluding test and extras dependencies.
- *raw_dependencies*: JSON list of dependencies, an extended 
    version of deps. This list includes package versions along
    with their names.

**raw_packages_info.csv**
is mostly important because of package URLs and licenses.

columns:
- package name,
- *author*: an author email, which can be useful to detect 
    packages owned by the same person.
- *license*: a license under which this package is released
- *url*: a github, bitbucket or gitlab URL.

**package_urls.csv**
This is a subset of URLs extracted from `raw_packages_info.csv`
with some extra filtering applied.

columns:
- *package name*: self-explaining 
- *URL*: a github url, e.g. `github.com/pandas-dev/pandas`

**backporting.csv**
 is a package x month dataframe containing 0 or 1 values, 
where 1 indicates that the package had a backport release in 
the given month.

**commits.csv**
 is a package x month dataframe containing number of commits 
 in the package repository in a given month.

**contributors.csv**
 is a package x month dataframe reflecting how many unique
 GitHub users commited to the package repository in a given 
 month.

**q90.csv**
 is a package x month dataframe representing how many people 
 were responsible for 90% of the contributions in the package
 repository.

**issues.csv**
 is a package x month dataframe containing number of issues
 reported in the package repository in a given month.

**non_dev_issues.csv**
 is the same as `issues.csv`, except not counting issues 
 reported by prior project committers.

**submitters.csv**
 is a package x month dataframe reflecting how many unique
 GitHub users reported issues to the package repository in 
 a given month.

**non_dev_submitters.csv**
 is the same as `submitters.csv` except not including 
 reporters who were committing to this project before

**university.csv**
 is a package x month dataframe representing what share of
 commits was made using university-affiliated addresses.

**commercial.csv**
 is the same as `university.csv` but representing share of
 commits made from organizational domains.

**cc_degree.csv**
 is a package x month dataframe representing how many projects
 did this project contributors also contribute in a given month.
 In the paper this metric was referred as social ties, as it
 was defined so in prior research.

**upstreams.csv**
 is a package x month dataframe representing how many projects
 did this project depend on in a given month.

**downstreams.csv**
 is a package x month dataframe representing how many projects
 depended on this project in a given month.

**d_upstreams.csv**
 is a package x month dataframe representing how many dead
 upstreams did this project have in a given month.
 An upstream is declared dead if it had less than 12 commits in 
 the last year.

**dc_katz.csv**
 is a package x month dataframe representing Katz centrality
 of a package in a network dependencies graph in a given month.

**github_user_info.csv**
 contains information about GitHub accounts under which project
 is hosted.
 
columns:
- *name*: name of the GitHub account (not a project name)
- *created_at*: ISO timestamp of when this accont was created
- *followers*: number of GitHub followers
- *following*: number of users followed by the account
- *org*: "True" if this user is an organization account,
 "False" otherwise
- *public_repos*: number of public repositories hosted under
 this account

**survival_data.csv**
contains a longitudinal dataset of PyPI projects and their 
metrics. 
Each observation except boolean variables 
(dead, backporting, organizational account)
represents average over the last 6 month.
Observations follow in 6 month periods, with the exception of 
the last one, observed either at project death or current date.
Projects considered dead are not observed anymore, even if they
are revived later.

- unique observation index
- *name*: project name
- *month*: YYYY-MM-DD date of the observation
- *commits*: number of commits in the project repository
- *contributors*: number of contributors in the project repository.
    Note this is an average of monthly contributors, not number
    contributors in the last 6 month.
- *q90*: size of the core team
- *issues*: number of reported issues
- *non_dev_issues*: same as issues, subtracted number of issues 
    reported by developers
- *submitters*: number of unique issues reporters
- *non_dev_submitters*: same as `submitters`, 
    with prior committers excluded.
- *upstreams*: number of upstream project (technical dependencies)
- *d_upstreams*: number of dead upstreams a project has
- *t_upstreams*: number of transitive upstream dependencies
- *downstreams*: number of downstream projects (i.e. projects
    technically dependent on this one)
- *t_downstreams*: number of projects transitively dependent on
    this one
- *dc_katz*: Katz centrality of a package in dependencies 
    network graph in a given month
- *dc_closeness*: closeness centrality of a package in dependencies 
    network graph in a given month
- *backporting*: 1 if this project issued a backport release in 
    the last year
- *cc_degree*: number of projects contributors of this project
    also committed to in a given month.
- *university*: share of commits from university-affiliated addresses
- *commercial*: share of commits from "commercial" addresses
- *age*: project age in month, zero-based
- *dead*: 1 if the project is considered dead (less than 12
    commits in the last year), 0 otherwise.
    Observations are stopped once project death is observed.
- *org*: 1 if the project is hosted under an organizational
    account on GitHub.
- *license*: License class
- *last_observation*: 1 if this is the last observation of the 
    project, 0 otherwise
- *license_code*: 1 for viral licenses like GPL,
    2 for semi-viral, 3 for non-viral


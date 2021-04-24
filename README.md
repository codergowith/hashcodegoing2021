Soware engineering at scale
Problem statement for the World Finals of Hash Code 2021
Introduction
Google stores the vast majority of its code in one monolithic codebase, holding
billions of lines of code. As of 2016, more than 25,000 soware engineers were
contributing to it
1
, improving existing services and adding cool new features to
meet the demands of their users.
With the growing size of the codebase and the increasing number of Google
soware engineers, it is geing challenging to schedule the engineers' work, so
they can be productive and launch features that make users happy.
Goal
Given a number of services, a number of binaries, and a set of features to
implement, decide how engineers should schedule their work to deliver features
that would delight the users as much as possible.
Problem description
Architecture
In this problem, we consider features, services and binaries:
● Features are the user-facing functionality of Google products. For example:
video playlists in YouTube or weather forecasts in Google Search.
● Services are programs running in Google data centers. For example:
YouTube may have a service responsible for storing user video collections.
● Binaries are groups of services which are combined to run together for
eciency. For example: services running in the same binary can share
resources such as a database connection.
1 hps://dl.acm.org/doi/10.1145/2854146
1
Features
Each feature is described by:
● The services it relies on.
● The number of daily users that benet from the feature (when it’s launched).
Once a feature is implemented in all services that it relies on, it is immediately
launched and users sta to benet from it.
Services and binaries
Each service is pa of exactly one binary, but one binary can contain multiple
services
2
. Services can be moved between binaries and new binaries can be
created as needed.
When implementing a feature, the engineers take advantage of the fact that a
binary contains multiple services - an engineer can implement a given feature in all
relevant services in a given binary at once.
Figure 1. There are two features (foo and bar), ve services (sa, sb, sc, sd, se), three
binaries (0, 1 and 2), and 2 engineers (G0 and G1). Binary 0 runs one service (se),
binary 1 runs two (sa and sc) and binary 2 also runs two (sb and sd). To implement
feature foo engineers need to work on services sb, sc, and sd, which means that
they will need to work on two binaries (1 and 2). To implement feature bar, engineers
need to work on features sa and sc, thus only binary 1.
2
Initially each binary contains at least one service, but it is valid for binaries to
become empty later on.
2
Scheduling work
Your goal is to assign work to engineers to implement features so that as many
users as possible benet from them.
Google has G engineers who can implement the features. On each day an engineer
can sta any of the following tasks:
● Implement a feature Fi
in the services of binary Bj
.
● Move service Si from binary Bj to another binary Bk
.
● Create a new empty binary.
● Wait for a number of days.
None of these tasks can be interrupted, meaning that once an engineer stas
working on a task, they will continue to do so for the duration of that task.
Making changes to a set of services that are running in one binary is easier than
making changes to services that are scaered across multiple binaries. On the
other hand, many engineers working on services in one binary get in each other’s
way, slowing down the work. This is described in more detail below.
Task: Implement a feature
An engineer can choose any binary Bj and implement a feature Fi
in all relevant
services in that binary at once.
If an engineer works on a binary Bj to implement a feature Fi
, this work will require
DFi + RBj + CBj days, where:
● DFi
is the diculty of feature Fi
,
● RBj
is the total number of services in Bj (including services not relevant to Fi),
and
● CBj
is the number of engineers already working on features in Bj on the rst
day of this work.
Once the task is done, feature Fi
is implemented in all relevant services in Bj
.
Multiple engineers can implement the same feature in dierent binaries, but only
one engineer at a time can implement a specic feature in one binary. If two
engineers sta working on the same binary (implementing two dierent features)
on the same day, the one listed earlier in the submission le is the rst to sta and
counts towards the number of engineers working on the binary for the second one.
3
(a) (b)
(c) (d)
Figure 2. (a) Engineer G0 works on binary 2 to implement feature foo. In this case,
RB2 = 3 as there are three services (sb, sc and sd) in binary 2, and CB2 = 0, as no other
engineer is working on that binary that day. In total, the engineer needs Dfoo + 3 + 0
days of work in binary 2 for feature foo. Similarly, engineer G1 needs Dbar + 1 + 0,
where 1 is the number of services in binary 1 (service sa).
(b) Both engineers work on binary 2 implementing dierent features. Supposing
that engineer G0 stas rst on this binary, G0 needs Dfoo + 3 + 0 days: since no-one
was working on binary 2, CB2 = 0. Engineer G1 needs Dbar + 3 + 1 days.
(c) It is possible for two engineers to work on the same feature (here feature bar),
but only if they work in two dierent binaries.
(d) It is not allowed for two engineers to work on the same feature on the same
binary.
4
Task: Move a service
An engineer can move a service Si from one binary Bj to a dierent binary Bk
. Aer
the move, all features that needed service Si will need to have engineers working
on binary Bk
instead of Bj
. Features already implemented in service Si remain
implemented.
It takes max(RBj
, RBk) days to move service Si from binary Bj to Bk
, where RBj and RBk
are the number of services running in binaries Bj and Bk respectively, before the
move.
No other engineer can sta working on binaries Bj and Bk until the move is
nished and the move can't sta if any engineer is working on either binary at
the time (implementing a feature or moving a service to/from one of these
binaries).
Figure 3. Engineer G0 moves service sc from binary 1 to binary 2. Observe that this
aects both features (foo and bar).
Task: Create a new binary
An engineer can spend N days to
create a new empty binary. Its ID is
the minimum positive integer that is
not used yet.
Figure 4. Engineer G1 creates a new
empty binary (binary 3).
5
Task: Wait
An engineer can wait for a number of days.
Figure 5. An engineer takes a break and
waits, for example watching cat videos.
Input data set
File format
Each input data set is provided in a plain text le. The le contains only ASCII
characters with lines ending with a single '\n' character (also called “UNIX-style” line
endings). When multiple numbers or strings are given in one line, they are separated
by a single space between each two elements.
● The rst line:
○ time limit in days: L (1 ≤ L ≤ 10
3
),
○ number of Google engineers G (1 ≤ G ≤ 10
5
),
○ number of services S (1 ≤ S ≤ 10
4
),
○ number of initial binaries B (1 ≤ B ≤ 10
4
),
○ number of features F (1 ≤ F ≤ 10
4
),
○ duration in days to create a new binary N (1 ≤ N ≤ 10).
● The next S lines describe the services and each of these contains:
○ the service name (string of 1-20 lowercase leers a-z and hyphens -),
○ an integer Bi
(0 ≤ Bi ≤ B-1) - the ID of the binary where the service
initially runs. Binaries are numbered from 0 to B-1.
● The next F blocks of lines describe the features. On each block:
○ The rst line contains:
■ the feature name (string of 1-20 lowercase leers a-z and
hyphens -),
■ Mi
(1 ≤ Mi ≤ S) - the number of services that need to be modied
to suppo the i-th feature,
■ Di
(1 ≤ Di ≤ 10
2
) - the diculty of the i-th feature,
■ Ui
(1 ≤ Ui ≤ 10
5
) - number of daily users that will benet from the
feature once it is launched.
○ The second line contains a list of strings Si,1
, Si,2
, ..., Si,Mi - the
names of the services to be modied to suppo the i-th feature.
6
Example
The following example input dataset matches the one shown in Figure 1.
10 2 5 3 2 5
sa 1
sb 2
sc 1
sd 2
se 0
foo 3 3 100
sc sb sd
bar 2 1 20
sc sa
10 days, 2 engineers, 5 services, 3 binaries and 2 features,
we need 5 days to create a new binary.
The first service is named 'sa' and runs in binary 1
The second service is named 'sb' and runs in binary 2
The third service is named 'sc' and runs in binary 1
The fourth service is named 'sd' and runs in binary 2
The fifth service is named 'se' and runs in binary 0
foo feature is implemented in 3 services, its difficulty is 3
and 100 users per day will benefit from it
foo feature is implemented in services sc, sb and sd
bar feature is implemented in 2 services, its difficulty is 1
and 20 users per day will benefit from it
bar feature is implemented in services sc and sa
Note that the input le does not contain any blank lines. Blank lines and line
wrapping in the example above are added for clarity.
Submissions
Your submission describes the schedule of the engineers' work.
File format
● The rst line: E - the number of engineers that we plan the work for (0 ≤ E ≤ G)
● The next E blocks:
○ The rst line: T (1 ≤ T ≤ L) - number of tasks for the given engineer.
○ The next T lines contain one of:
■ Literal impl followed by a feature name Fi and a number Bj -
the engineer should implement the feature named Fi
in the
binary with ID Bj
.
■ Literal move followed by service name Si and a number Bj - the
engineer should move service Si from binary Bk
(in which Si
resides at the time of the move) to a dierent binary Bj
.
■ Literal new - the engineer should sta a new (empty) binary.
■ Literal wait followed by number W (1 ≤ W ≤ L) - the engineer
should wait for W days.
7
Example
2
2
move sc 2
impl foo 2
3
wait 2
impl bar 1
impl bar 2
Both engineers will work
The first engineer will do 2 tasks
Move service sc to binary 2
Implement feature foo in binary 2
The second engineer will do 3 tasks
Wait for 2 days
Implement feature bar in binary 1
Implement feature bar in binary 2
Figure 6. The timeline of the engineers' work.
For engineer G0, moving cs from binary 1, where it is initially, to binary 2 will need
max(RB1
, RB2), so 2 days. Implementing foo in binary 2 will require 6 days: Dfoo + RB2 +
CB2
, where Dfoo = 3, RB2 = 3 and CB2 = 0. Similarly, for engineer G1 we can calculate the
days required to implement each of the binaries for the feature bar.
(a) (b)
8
(c)
Figure 7. The work of each engineer at every given time: (a) between days 0 and 2,
(b) between days 2 and 4, (c) between days 4 and 8. Aer day 8, feature foo goes
live, and stays live until the time limit (day 10), thus for 2 days. Feature bar goes live
aer day 9, and thus stays live for 1 day.
Scoring
The engineers execute the scheduled tasks, one immediately aer another,
according to the submission le. If an engineer is scheduled to do a task but they
can’t (e.g. they cannot move a service because another engineer is still working on
the same binary), the solution is considered invalid and gets 0 points.
It is valid for an engineer to nish their tasks before the time limit. It is also valid to
schedule tasks that will sta or nish aer the time limit (such tasks are ignored).
Once a feature is implemented in all relevant services before the time limit, it is
immediately launched and the users sta to benet from it. It is valid to only
paially implement a feature (e.g. if the engineer working on it would nish aer
the time limit or not even sta implementing the feature in some binaries), but it
won’t earn any points. Likewise, features that get fully implemented aer the time
limit are allowed but won't earn any points.
9
Each feature launched before the time limit scores a number of points equal to
Ui ✕ max(0, L - Ii
)
where
● Ui - number of users that benet from the i-th feature
● L - time limit in days
● Ii - the day when the i-th feature was launched (number of days it took to
fully implement i-th feature).
The total score is a sum of the scores earned by each launched feature.
Example
For instance, in the example above, the rst engineer will spend two days to move
service sc from binary 1 to binary 2: initially there are 2 services in binary 1, and 2
services in binary 2, so max(2, 2) = 2. The rst engineer will then spend 6 days (3 + 3
+ 0) to implement feature foo there. So foo will be ready on day 8, meaning it will be
live for two days (until day 10, which is the time limit), scoring 200 (= 2 days ✕ 100
users) points.
The second engineer will spend 2 days waiting and then 2 days (1 + 1 + 0)
implementing feature bar in binary 1. They will then spend 5 days (1 + 3 + 1)
implementing feature bar in binary 2. The feature will be live for 1 day, scoring 20
points.
The nal score is 220 (200 + 20) points.
Note that there are multiple data sets representing separate instances of the
problem. The nal score for your team will be the sum of your best scores for
the individual data sets.
10

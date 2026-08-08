<p align="center">
  <h3 align="center">Perl Style Guide</h3>
  <p align="center">A starting point for Perl development teams to provide consistency through good practices</p>
</p>

---

#### Summary

This paper is summarized in some good practice guidelines for Perl coding using "post-modern" practices. This work is in progress and any suggestions/contributions are welcome.

This project is just one of several other coding style guides, there is no intention of setting a pattern from it. Please do not take this as absolute truth. The most important thing here is that you and your team feel comfortable with a certain guideline and make use of it.

This paper is a fork of the paper written by [Eric Lorenzana](https://github.com/chusqui/perl-style-guide), with only a few changes in the sense of organization of the material and some adjustments to current market practices. 

---

#### Code layout

- Do not write any comments in the code. Your code must be self-explanatory without comments. [See the comments section.](#comments)

- All code must be written in English, using clear, descriptive, and intuitive variable, function, and method names.

- Make use of the "strict" and "warnings" modules in all your codes, they provided you:
    1. strict: It forces you to code properly to make your program less error-prone. For example: It forces you to declare variables before you use them. You can declare variable using “my” keyword. “my” keyword restricts the scope of the variable to local. It makes the code more readable and less error prone. If you don’t declare variable using my keyword then the created variable would be global, which you should avoid, reducing the scope of the variable to the place where it is needed is a good programming practice.

    2. warnings: It helps you find typing mistakes, it warns you whenever it sees something wrong with your program. It would help you find mistakes in your program faster.


- Try to limit your code to 72-78 column lines...
  - But don't stress over it. Real world code often has very long sentences, and trying to force them to be below 78 columns leads to tight, but ugly code. In a nutshell: <= 72 is perfect, <= 78 is great, > 78 is not as bad as some folks might try to make you believe.
  
- Use four spaces per indentation level. The main advantage being that it provides a consistent and readable format, making it easier to maintain code quality. This level of indentation also helps in keeping lines within a reasonable length, promoting better readability.

- Do not use tabs. It makes code difficult to browse in some hosts (where 8 spaces per tab is the standard) and only works when indenting by blocks.

---

#### Comments

Comments are not tolerated. A comment is a confession that the code failed to explain itself, and the fix is always the code, never the comment. Code is verified by the interpreter and by your tests; comments are verified by nobody. They rot silently, drift away from what the code actually does, and eventually lie to the next person reading them.

- Do not write comments. Rewrite the code until the comment is unnecessary.

- Never restate what the code already says.

```perl
# Bad
# increment the counter
$counter = $counter + 1;

# Good
$counter = $counter + 1;
```

- When you feel the urge to explain a value, name the value.

```perl
# Bad
# 86400 is the number of seconds in a day
if ($elapsed > 86400) {
  ...
}

# Good
my $seconds_in_a_day = 86400;

if ($elapsed > $seconds_in_a_day) {
  ...
}
```

- When you feel the urge to explain a block, extract it into a subroutine whose name is the explanation.

```perl
# Bad
# check whether the user is allowed to read the report
if ($user -> role eq "admin" || $user -> id == $report -> owner_id) {
  ...
}

# Good
if (user_can_read_report($user, $report)) {
  ...
}
```

- Never leave commented-out code behind. Delete it. Version control already remembers it, and dead code that looks alive is worse than no code at all.

```perl
# Bad
# my $result = old_implementation($input);
my $result = new_implementation($input);

# Good
my $result = new_implementation($input);
```

- Do not write banners, separators, author tags, dates, or changelogs in the source. All of this belongs to your version control system, where it stays accurate.

```perl
# Bad
##########################################
# Author: someone
# Created: 2024-01-01
# Changelog: fixed the parser
##########################################
```

- Do not write `TODO`, `FIXME`, or `HACK` notes. Pending work belongs in your issue tracker, where it can be assigned, prioritized, and closed. A note buried in the source is work nobody agreed to do.

- Do not explain a subroutine with a comment. If the name does not tell the whole story, the name is wrong or the subroutine does too much.

```perl
# Bad
# returns the user name and, if missing, the email
sub get_user {
  ...
}

# Good
sub user_name_or_email {
  ...
}
```

- If a piece of code needs justification, such as a workaround for an upstream bug or a deliberate deviation, that justification is history, not code. Write it in the commit message and in the issue that tracks it.

- Note that shebang lines and compiler directives are not comments, even though they start with `#`. They are instructions, they are read by the machine, and they stay.

```perl
#!/usr/bin/env perl
## no critic (ProhibitStringyEval)
```

- Note as well that POD is documentation, not commentary. A distributable module describes its public interface in POD, aimed at whoever consumes the module without reading the source. That is a contract for outsiders, and it never becomes an excuse to narrate the implementation from inside.

---
  
#### Good practices  

```perl
# Don't do this:
my $string =
  "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor aliqua.";

# Do this instead:
my $string = "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor aliqua."

my $fn = sub {$_[0] + 1};              # Bad
my $fn = sub { $_[0] + 1 };            # Good
```

* Don't cuddle an else.

```perl
# Good
if (condition) {
  ...
}

else {
  ...
}

# Bad
if (condition) {
  ...
} else {
  ...
}
```

```perl
# Good
sub function {
  if (test) {
    ...
  }
}

# Bad
sub function
{
  if (test)
  {
    ...
  }
}
```

- Always unpack the stack first.
- Do not, ever, modify the stack. Unless necessary.

```perl
# Bad
sub my_method {
  my $self = shift;
  my %params = @_;
  ...
}

# Good
sub my_method {
  my ($self, %params) = @_;
  ...
}
```

- Do not put spaces between spaces, braces and brackets. That is, arrays, hashes, array and hash references, string and command line delimiters.

```perl
my ( $self, $who, @params ) = @_;      # Bad
my ($self, $who, @params) = @_;        # Good

my $self = { name => "Eric", age => 26 };  # Bad
my $self = {name => "Eric", age => 26};    # Good

my @files = qx| ls $str |;             # Bad
my @files = qx|ls $str|;               # Good
```

- Please keep in mind that this does not apply to operators.

```perl
my $fn = sub {$_[0] + 1};              # Bad
my $fn = sub { $_[0] + 1 };            # Good

my @items = map {do_something_to $_} @_;    # Bad
my @items = map { do_something_to $_ } @_;  # Good
```

- Use whitespace between operators.

```perl
# Good
my $area = $pi * ($radius ^ 2);

# Bad
my $area = $pi*($radius^2);
```

- Always keep a space before and after the `->` operator.

```perl
my $name = $user->name;               # Bad
my $name = $user ->name;              # Bad
my $name = $user-> name;              # Bad
my $name = $user -> name;             # Good
```

- Do not use ternary operators; use explicit control flow blocks instead.

```perl
# Bad
my $status = $ok ? "ready" : "not ready";

# Good
my $status;
if ($ok) {
  $status = "ready";
}
if (!$ok) {
  $status = "not ready";
}
```

- Avoid using `else`; prefer early returns or separate conditional blocks.

```perl
# Bad
if ($valid) {
  return 1;
} else {
  return 0;
}

# Good
if ($valid) {
  return 1;
}
return 0;
```

- Do not use post-if conditions (e.g., `statement if condition`); always use explicit conditional blocks.

```perl
# Bad
log_event($event) if $event;

# Good
if ($event) {
  log_event($event);
}
```

- Associate hash key and values by using fat comma.

```perl

# Correct, but bad style
my %hash = (key1, 'val1',
            key2, 'val2',
            key3, 'val3');

# Good
my %hash = (
              key1 => 'val1',
              key2 => 'val2',
              key3 => 'val3'
           );
```

---

#### Recommended modules

- Use `Readonly` for constants and immutable values.

```perl
use Readonly;
Readonly my $PI => 3.1415926;
```

- Use `English` to refer to Perl's built-in variables by name instead of by punctuation. Punctuation variables force the reader to keep a lookup table in their head, and this guide expects code to explain itself.

```perl
use English qw(-no_match_vars);
```

```perl
# Bad
print $0;
print $!;
print $^O;
print $@;

# Good
print $PROGRAM_NAME;
print $ERRNO;
print $OSNAME;
print $EVAL_ERROR;
```

- Always import it with `-no_match_vars`. Importing the match variables (`$MATCH`, `$PREMATCH`, `$POSTMATCH`) used to slow down every regular expression in the program, and while modern Perl no longer pays that cost, the explicit import states that you do not want them and keeps the intent unambiguous.

---

#### Dependencies

- Declare every dependency in a `cpanfile` at the root of the project. A dependency that lives only in someone's shell history, in a README instruction, or in the memory of whoever set up the server is not a declared dependency.

- Pin exact versions with `==`. A floating version means the code you ship is not the code you tested, and the day it breaks will be a day when nothing in your repository changed.

```perl
# Bad
requires "Readonly";
requires "Try::Tiny", ">= 0.30";

# Good
requires "Readonly", "== 2.05";
requires "Try::Tiny", "== 0.31";
```

- Separate dependencies by phase, so that a production install does not drag in your test and profiling tools.

```perl
requires "Readonly", "== 2.05";
requires "Try::Tiny", "== 0.31";

on "test" => sub {
  requires "Test::More", "== 1.302195";
};

on "develop" => sub {
  requires "Devel::NYTProf", "== 6.14";
};
```

- Commit both `cpanfile` and `cpanfile.snapshot`. Pinning your direct dependencies does not pin theirs, and the snapshot produced by `carton install` locks the whole resolved tree. Without it, "pinned" only describes the first level.

- Install from the declaration, never by hand. `carton install` or `cpanm --installdeps .` guarantees that every machine builds the same tree from the same file. Anything installed manually exists on exactly one machine and disappears with it.

---

#### Performance tips

##### Profiling

- Profile your code with `Devel::NYTProf`.

---

#### Contribution

- Your contributions and suggestions are heartily ♥ welcome. [See here the contribution guidelines.](https://github.com/lesis-lat/perl-style-guide/blob/master/.github/CONTRIBUTING.md) Please, report bugs via [issues page.](https://github.com/lesis-lat/perl-style-guide/issues)  and for security issues see here the [security policy.](https://github.com/lesis-lat/perl-style-guide/blob/master/SECURITY.md) (✿ ◕‿◕) 

---

#### License

- This work is licensed under [MIT License.](https://github.com/lesis-lat/perl-style-guide/blob/master/LICENSE.md)

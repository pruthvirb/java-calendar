# Violation Comments to GitHub Maven Plugin [![Build Status](https://travis-ci.org/tomasbjerre/violation-comments-to-github-maven-plugin.svg?branch=master)](https://travis-ci.org/tomasbjerre/violation-comments-to-github-maven-plugin) [![Maven Central](https://maven-badges.herokuapp.com/maven-central/se.bjurr.violations/violation-comments-to-github-maven-plugin/badge.svg)](https://maven-badges.herokuapp.com/maven-central/se.bjurr.violations/violation-comments-to-github-maven-plugin) [ ![Bintray](https://api.bintray.com/packages/tomasbjerre/tomasbjerre/se.bjurr.violations%3Aviolation-comments-to-github-maven-plugin/images/download.svg) ](https://bintray.com/tomasbjerre/tomasbjerre/se.bjurr.violations%3Aviolation-comments-to-github-maven-plugin/_latestVersion)

This is a Maven plugin for [Violation Comments to GitHub Lib](https://github.com/tomasbjerre/violation-comments-to-github-lib).

It can be used in Travis, or any other build server, to read results from static code analysis and comment pull requests in GitHub with them.

You can have a look at [violations-test](https://github.com/tomasbjerre/violations-test/pull/2) to see what the result may look like.

The merge must be performed in order for the commented lines in the PR to match the lines reported by the analysis tools!

It supports:
 * [_AndroidLint_](http://developer.android.com/tools/help/lint.html)
 * [_Checkstyle_](http://checkstyle.sourceforge.net/)
   * [_Detekt_](https://github.com/arturbosch/detekt) with `--output-format xml`.
   * [_ESLint_](https://github.com/sindresorhus/grunt-eslint) with `format: 'checkstyle'`.
   * [_KTLint_](https://github.com/shyiko/ktlint)
   * [_SwiftLint_](https://github.com/realm/SwiftLint) with `--reporter checkstyle`.
   * [_PHPCS_](https://github.com/squizlabs/PHP_CodeSniffer) with `phpcs api.php --report=checkstyle`.
 * [_CLang_](https://clang-analyzer.llvm.org/)
   * [_RubyCop_](http://rubocop.readthedocs.io/en/latest/formatters/) with `rubycop -f clang file.rb`
 * [_CodeNarc_](http://codenarc.sourceforge.net/)
 * [_CPD_](http://pmd.sourceforge.net/pmd-4.3.0/cpd.html)
 * [_CPPLint_](https://github.com/theandrewdavis/cpplint)
 * [_CPPCheck_](http://cppcheck.sourceforge.net/)
 * [_CSSLint_](https://github.com/CSSLint/csslint)
 * [_DocFX_](http://dotnet.github.io/docfx/)
 * [_Findbugs_](http://findbugs.sourceforge.net/)
 * [_Flake8_](http://flake8.readthedocs.org/en/latest/)
   * [_AnsibleLint_](https://github.com/willthames/ansible-lint) with `-p`
   * [_Mccabe_](https://pypi.python.org/pypi/mccabe)
   * [_Pep8_](https://github.com/PyCQA/pycodestyle)
   * [_PyFlakes_](https://pypi.python.org/pypi/pyflakes)
 * [_FxCop_](https://en.wikipedia.org/wiki/FxCop)
 * [_Gendarme_](http://www.mono-project.com/docs/tools+libraries/tools/gendarme/)
 * [_GoLint_](https://github.com/golang/lint)
   * [_GoVet_](https://golang.org/cmd/vet/) Same format as GoLint.
 * [_JSHint_](http://jshint.com/)
 * _Lint_ A common XML format, used by different linters.
 * [_JCReport_](https://github.com/jCoderZ/fawkez/wiki/JcReport)
 * [_Klocwork_](http://www.klocwork.com/products-services/klocwork/static-code-analysis)
 * [_MyPy_](https://pypi.python.org/pypi/mypy-lang)
 * [_PerlCritic_](https://github.com/Perl-Critic)
 * [_PiTest_](http://pitest.org/)
 * [_PyDocStyle_](https://pypi.python.org/pypi/pydocstyle)
 * [_PyLint_](https://www.pylint.org/)
 * [_PMD_](https://pmd.github.io/)
   * [_Infer_](http://fbinfer.com/) Facebook Infer. With `--pmd-xml`.
   * [_PHPPMD_](https://phpmd.org/) with `phpmd api.php xml ruleset.xml`.
 * [_ReSharper_](https://www.jetbrains.com/resharper/)
 * [_SbtScalac_](http://www.scala-sbt.org/)
 * [_Simian_](http://www.harukizaemon.com/simian/)
 * [_StyleCop_](https://stylecop.codeplex.com/)
 * [_XMLLint_](http://xmlsoft.org/xmllint.html)
 * [_ZPTLint_](https://pypi.python.org/pypi/zptlint)

 
## Usage ##
There is a running example [here](https://github.com/tomasbjerre/violation-comments-to-github-maven-plugin/tree/master/violation-comments-to-github-maven-plugin-example).

Here is and example: 

```
	<plugin>
		<groupId>se.bjurr.violations</groupId>
		<artifactId>violation-comments-to-github-maven-plugin</artifactId>
		<version>1.28</version>
		<executions>
			<execution>
				<id>ViolationCommentsToGitHub</id>
				<goals>
					<goal>violation-comments</goal>
				</goals>
				<configuration>
					<username>${GITHUB_USERNAME}</username>
					<password>${GITHUB_PASSWORD}</password>
					<oAuth2Token>${GITHUB_OAUTH2TOKEN}</oAuth2Token>
					<pullRequestId>${GITHUB_PULLREQUESTID}</pullRequestId>
					<repositoryOwner>tomasbjerre</repositoryOwner>
					<repositoryName>violations-test</repositoryName>
					<gitHubUrl>https://api.github.com/</gitHubUrl>
					<createCommentWithAllSingleFileComments>false</createCommentWithAllSingleFileComments>
					<createSingleFileComments>true</createSingleFileComments>
					<commentOnlyChangedContent>true</commentOnlyChangedContent>
					<!-- INFO, WARN, ERROR //-->
					<minSeverity>INFO</minSeverity>
					<violations>
						<violation>
							<parser>FINDBUGS</parser>
							<reporter>Findbugs</reporter>
							<folder>.</folder>
							<pattern>.*/findbugs/.*\.xml$</pattern>
						</violation>
						<violation>
							<parser>PMD</parser>
							<reporter>PMD</reporter>
							<folder>.</folder>
							<pattern>.*/pmd/.*\.xml$</pattern>
						</violation>
						<violation>
							<parser>CHECKSTYLE</parser>
							<reporter>Checkstyle</reporter>
							<folder>.</folder>
							<pattern>.*/checkstyle/.*\.xml$</pattern>
						</violation>
						<violation>
							<parser>JSHINT</parser>
							<reporter>JSHint</reporter>
							<folder>.</folder>
							<pattern>.*/jshint/.*\.xml$</pattern>
						</violation>
						<violation>
							<parser>CSSLINT</parser>
							<reporter>CSSLint</reporter>
							<folder>.</folder>
							<pattern>.*/csslint/.*\.xml$</pattern>
						</violation>
					</violations>
				</configuration>
			</execution>
		</executions>
	</plugin>
```

To send violations, just run:
```
mvn violation-comments-to-github-maven-plugin:violation-comments -DGITHUB_PULLREQUESTID=$GITHUB_PULL_REQUEST -DGITHUB_USERNAME=... -DGITHUB_PASSWORD=...
```

Or if you want to use OAuth2:
```
mvn violation-comments-to-github-maven-plugin:violation-comments -DGITHUB_PULLREQUESTID=$GITHUB_PULL_REQUEST -DGITHUB_OAUTH2TOKEN=$GITHUB_OAUTH2TOKEN
```

You may also have a look at [Violations Lib](https://github.com/tomasbjerre/violations-lib).

## Developer instructions

To make a release, first run:
```
mvn release:prepare -DperformRelease=true
mvn release:perform
```
Then release the artifact from [staging](https://oss.sonatype.org/#stagingRepositories). More information [here](http://central.sonatype.org/pages/releasing-the-deployment.html).

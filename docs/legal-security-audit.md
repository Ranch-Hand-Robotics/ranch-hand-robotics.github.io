# Legal and security audit — 2026-10-11

## Scope and limits

Reviewed website source, templates, configuration, workflow, tracked dependency lockfile, current tracked text for high-confidence credential patterns, and an isolated Jekyll build. This is a technical/disclosure audit, not a legal opinion or a compliance certification.

Not verified: Git history, binary metadata, every secret format, private repository contents, GitHub administration settings, Microsoft Forms account settings or response storage, DNS ownership, live production response headers, or the dependency inventory inside GitHub's build container. No deployment or account settings were changed.

## Gem-update follow-up — 2026-10-11

Updated or added 48 gems after raising the GitHub Pages constraint from 228 to 232. The installed bundle now includes Jekyll 3.10.0, Minimal Mistakes 4.28.1, and Nokogiri 1.19.4. `bundle check` and a full site build passed under Ruby 3.2.3.

A fresh OSV query of all 100 locked RubyGems packages found **one remaining affected package: rubyzip 2.4.1**, matching GHSA-47m2-wp7j-p9vc. The other eight affected packages from the original audit no longer matched advisories. The current GitHub Pages 232 meta-gem still pins `jekyll-remote-theme` 0.4.3, which requires Rubyzip below 3; resolving the remaining advisory requires changing that dependency/build strategy rather than forcing an incompatible version.

The workflow's separate build-image inventory remains unverified. Updating the local lockfile does not update that container or the independently pinned remote-theme release in `_config.yml`.

## Fixes made

- Mobile hero grows to fit content and reserves space for the scroll prompt. Verified at 320, 390, 1024, and 1440 pixels.
- White-text buttons use a darker green gradient; tested minimum contrast is approximately 5.1:1.
- Six sponsor-supported cards now provide separate Learn more and Sponsor links. Added HarvestHive and Robots for ATAK detail pages; no nested interactive links.
- Both navigation configurations point to real Available Today and Sponsor Preview anchors.
- Removed the shared robot illustration from Sheepdog and AI Ground Station. Mule's remaining illustration is explicitly labeled as a concept, not a tested prototype.
- Replaced the automatically loaded Microsoft Forms iframe with an opt-in external link, an integration notice, a privacy-statement link, and a warning not to submit secrets.
- Added referrer policy metadata. Added Git ignores and explicit publication exclusions for environment/key files and development artifacts. Exclusions are not a substitute for keeping secrets out of Git.
- Restricted Pages-write/OIDC permissions to the deployment job. Pinned five action references to verified upstream commits; configured weekly Bundler and Actions Dependabot updates.
- Replaced the theme's unversioned Font Awesome `@latest` CSS dependency with release 6.7.2, verified SHA-384 integrity, anonymous CORS, and no-referrer loading. Preserved the upstream MIT notice in the local theme-head override. Theme pages still request CSS/fonts from jsDelivr; this is not a claim of zero third-party data processing.
- Added private security-reporting guidance in `SECURITY.md`.

## Open security findings

### High priority: remaining Rubyzip advisory; original baseline below

The original audit queried OSV's live RubyGems advisory database for all 98 package/version pairs parsed from the previous `Gemfile.lock`. Nine locked packages matched advisories. The table below preserves that baseline, not the current dependency inventory; see the gem-update follow-up above for the current single affected package. These are build/local-preview dependencies; an affected version does not establish an exploitable public-site endpoint.

| Locked dependency | Version | Representative advisory |
| --- | --- | --- |
| activesupport | 7.1.5.1 | GHSA-2j26-frm8-cmj9 — number-helper denial of service |
| addressable | 2.8.7 | GHSA-h27x-rffw-24p4 — template regular-expression denial of service |
| concurrent-ruby | 1.3.5 | GHSA-6wx8-w4f5-wwcr — lock-state corruption |
| faraday | 2.12.2 | GHSA-33mh-2634-fwr2 — protocol-relative host override/SSRF |
| json | 2.9.1 | GHSA-x2f5-4prf-w687 — IO-streaming generator buffer overflow |
| nokogiri | 1.18.8, Linux binary | GHSA-353f-x4gh-cqq8 — bundled libxml2 CVEs, classified critical by the advisory |
| rexml | 3.4.0 | GHSA-c2f4-jgmc-q2r5 — malformed-XML denial of service |
| rubyzip | 2.4.1 | GHSA-47m2-wp7j-p9vc — archive path traversal |
| uri | 1.0.3 | GHSA-j4pr-3wm6-xx2r — credential leakage |

The table is representative, not the complete advisory set or a safe-version upgrade matrix. OSV returned additional advisories for Active Support, Concurrent Ruby, Faraday, and Nokogiri. Re-query all versions after any update; fixing the oldest advisory alone is insufficient.

Next action: resolve the remaining Rubyzip/build constraint and rerun the advisory scan. Do not claim this lockfile is vulnerability-free. `jekyll-remote-theme` 0.4.3 constrains `rubyzip` below 3, whereas the verified path-traversal advisory lists 3.4.0 as fixed. That requires a theme/build strategy decision, not a blind transitive version bump. Compatible gem updates were completed in the follow-up; the original audit itself did not change dependency versions.

### Medium priority: build supply-chain reproducibility remains partial

The workflow action references are immutable, but the pinned `actions/jekyll-build-pages` action itself references the tagged image `ghcr.io/actions/jekyll-build-pages:v1.0.13`. Its Dockerfile installs its own Gemfile; the local lockfile audit is not an audit of that image. The remote theme also uses a release tag rather than a verified commit.

Next action: inventory/scan the actual build image and consider digest-pinning or a lockfile-controlled build pipeline. Continue reviewing Dependabot changes rather than automatically merging them.

### Administration and response-header checks remain open

Verify custom-domain ownership and GitHub Pages **Enforce HTTPS**, branch/environment protections, secret scanning, alert settings, and sponsor repository access/removal rules. The repository cannot prove these settings. Public GitHub Pages artifacts are not an access-control mechanism for sponsor-only code or designs.

No enforced Content Security Policy, frame-ancestors policy, or comprehensive production-header assessment was added. Inline theme scripts require an inventory before deploying a restrictive policy. GitHub Pages hosting may require an additional serving layer for custom response headers; a repository `_headers` file alone would not enforce them.

## Open legal/disclosure decisions

### Privacy and form retention

The contact page now accurately identifies the external service and avoids loading the form automatically. This is not a complete site privacy policy. The owner must determine actual collected fields, retention/deletion practices, access controls, processing locations, and any legally applicable rights/notice requirements. Account settings and visitor jurisdictions determine obligations; this audit does not assert GDPR/CCPA/CASL applicability.

Third-party CSS/font requests and hosting logs must also be considered. Absence of a configured analytics tracker does not prove absence of personal-data processing or cookies at external destinations. Microsoft's statement does not replace the operator's own notice.

### Repository licensing and asset provenance

No root website `LICENSE` was found. Choose the intended license for original website code/content separately from assets and project repositories. Do not apply a blanket license to third-party logos, artwork, fonts, or sponsor-only materials without authority. Existing external projects may already have their own licenses; the site's missing license does not invalidate those licenses.

Verify rights and attribution requirements for `assets/images`, including concept artwork, the hero background, extension screenshots, and product logos. Removing an illustration from a page does not establish provenance or remove the asset from the repository. The Grok watermark is not sufficient evidence of commercial-use rights or any particular attribution requirement; no such requirement was invented here.

The copied theme-head template retains the verified Minimal Mistakes MIT notice. Font Awesome remains externally hosted; review its applicable CSS/font/icon licenses if bundling those assets later.

### Sponsorship offer clarity

Finalize and synchronize the $1 entry tier and $5 selected-repository benefit, if offered. Website copy still says $5 because that was the visible GitHub entry tier when checked. Explicitly identify which repositories qualify, access duration and removal conditions, and the support/refund/cancellation expectations consistent with the actual GitHub offering. Do not promise access to every repository or retroactively invent a refund policy.

The no-preorder/no-release-date wording is useful disclosure, not a substitute for reviewing the actual paid benefits. Sponsorship is not automatically a tax-deductible charitable donation. Avoid making that claim.

### Claims, marks, and robot safety

Verify the evidence and date for the 400k+ cumulative-install claim. Distinguish installs from unique users. Confirm open-source claims against each project's actual license and planned/public status. Product and third-party platform names should identify compatibility, not imply endorsement or certified safety.

Before publishing operational robot instructions or selling hardware, review prototype limitations, safety testing, regulatory requirements, and commercial terms with appropriate expertise. The current design-goal wording is not a safety certification or a waiver of product obligations.

## Verification evidence

- High-confidence credential-pattern scan: 26 current tracked text files, zero matches. Git history, binaries, less distinctive secret formats, and private repositories were not scanned.
- OSV batch query succeeded for 98 locked RubyGems dependencies; representative advisory detail queries succeeded for each affected package.
- Gem-update follow-up: all 100 current locked packages were rechecked; only Rubyzip remained affected. The updated bundle check, full build, and expected-page/artifact-exclusion checks passed.
- Five GitHub action commit references were resolved from their official upstream repositories; workflow assertions confirmed read-only global/build permissions and deployment-only Pages-write/OIDC permissions.
- Jekyll build succeeded under Ruby 3.2.3. Existing non-fatal pagination and Faraday retry warnings remain; they are not evidence of security vulnerabilities.
- Isolated artifact inspection confirmed no root development vendor directory, environment/key files, README, security report, workflow files, or development configuration were published. Browser-side theme assets under `assets/js/vendor` are intentional public files.
- Ten built pages and their internal links/referrer policies passed browser checks. No form iframe remained; no shared concept image remained on Sheepdog or AI Ground Station. Layout and contrast checks passed.

## Sources

- [OSV API](https://api.osv.dev/) — RubyGems package/version queries and advisory details; results are time-sensitive.
- [GitHub HTTPS guidance](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https).
- [Microsoft Privacy Statement](https://privacy.microsoft.com/en-us/privacystatement).
- [Pinned Jekyll action definition](https://github.com/actions/jekyll-build-pages/blob/44a6e6beabd48582f863aeeb6cb2151cc1716697/action.yml) and Dockerfile.

This document is excluded from the built public site via `_config.yml`; it remains visible wherever the repository itself is public.
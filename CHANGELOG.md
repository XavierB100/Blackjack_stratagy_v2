# CHANGELOG

## [0.1.0] – 2025-07-15

### Added
* Initial beta with working single-hand gameplay
* Perfect basic strategy decision engine
* Hi-Lo card counting system (running/true count)
* Visual strategy recommendation display
* Basic card animations and game notifications
* Win/loss statistics tracking
* Simple betting advice based on true count

## [0.2.0] – 2025-07-17

### Added
* Full multi-hand split functionality (up to 4 hands)
* Active hand highlighting in split scenarios
* Deck penetration tracking and display
* Special rules for splitting Aces
* Blackjack-specific win animation and tracking
* Fade-in animations for game messages
* Pulsing effects for important notifications

### Changed
* Redesigned card back appearance
* Improved true count calculation accuracy
* Enhanced mobile responsiveness
* Refactored game state management for multiple hands
* Updated betting advice with clearer indicators

### Fixed
* Corrected soft hand strategy calculations
* Fixed pair splitting rules (especially for 4s)
* Improved dealer soft 17 behavior
* Resolved card counting edge cases
* Fixed statistics tracking for split hands

## [0.2.1] – 2025-01-XX

### Fixed
* **CRITICAL**: Corrected card counting calculation - now uses proper deck rounding
* **CRITICAL**: Fixed A,7 strategy - now correctly stands vs 2-8, hits vs 9,10,A
* **CRITICAL**: Fixed 4s splitting rule - now correctly splits vs 5,6
* **CRITICAL**: Added proper reshuffle at 75% penetration (industry standard)
* **CRITICAL**: Fixed blackjack payout tracking - now properly tracks 3:2 payouts
* **CRITICAL**: Fixed dealer soft 17 rule implementation
* Enhanced statistics tracking with blackjack, double, and split wins
* Improved game state management for multiple hands
* Added dealer rules configuration system

### Changed
* Updated card counting to use industry-standard deck calculation
* Improved true count accuracy with proper rounding
* Enhanced statistics display with additional metrics
* Better error handling for edge cases

## [0.3.0] – 2025-01-XX

### Added
* **Bankroll management system** with $10,000 starting balance
* **Surrender option** for unfavorable hands (loses half bet)
* **Optimal bet sizing** based on true count
* **Hand history tracking** (last 50 hands)
* **Session statistics** with profit/loss tracking
* **Toggle bankroll display** for clean interface
* **Real-time bet adjustment** with count-based recommendations

## [0.4.0] – Planned

### Upcoming
* Strategy drill builder mode
* Advanced deviation training (Illustrious 18)
* Customizable rule sets
* Multiplayer tournaments and leaderboards
* AI coaching system with personalized feedback

This changelog highlights the significant improvements in Iteration 2, particularly the complex split hand functionality and enhanced visual feedback, while maintaining all the core features from the initial version. The upcoming features suggest a focus on training tools and advanced learning features in future updates.

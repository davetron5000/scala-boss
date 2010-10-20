!SLIDE
# Convincing the Boss

!SLIDE bullets incremental
# What we're really asking
* To *use* Scala, not replace Java
* Add a powerful tool to our toolbox
* Keep proven technologies around

!SLIDE bullets incremental
# The boss will want specifics:
* In-house expert (that's you)
* Well-chosen pilot project
* How you will minimize risk

!SLIDE bullets incremental
# Become the expert
* Prototype something
* weekend/20% time/innovation day

!SLIDE 
# Pilot Project

!SLIDE center
# Basic Team/Code Org Chart
<img src="team_structure.png">

!SLIDE bullets incremental
# Testing?
* No effect on production codebase
* Risks of new language more acceptible
* Small, constrained problem set

!SLIDE bullets incremental
# New (or refactored) subsystem?
* Separate from primary codebase
* High reward
* Smaller team, easier ramp up

!SLIDE bullets incremental
# New project/application?
* More risky
* Even higher reward
* Smaller team, easier ramp up

!SLIDE smbullets incremental
# How you will minimize risk:
## Judicious use of Scala features
* Start slowly
* OO design
* Keep to core library
* Avoid implicits and highly-terse internal DSLs

!SLIDE bullets incremental
# Be sure to use…
* package objects
* 2.8 collections library
* <code>Option</code>
* case classes

!SLIDE smaller
# Case class awesome
## Java (not so awesome)
    @@@ Java
    public class ResourceUsage implements TimeWindow {

        private final long fromDateMillis;
        private final long toDateMillis;
        private final BigDecimal usageValue;

        private final long durationSeconds;
        private String stringRepresentation;

        public ResourceUsage(long fromDateMillis, long toDateMillis, BigDecimal usageValue) {
            if (toDateMillis <= fromDateMillis) {
                throw new IllegalArgumentException(toDateMillis + " must be after " + fromDateMillis);
            }
            if (usageValue == null) {
                throw new IllegalArgumentException("cannot have null usage");
            }
            this.fromDateMillis = fromDateMillis;
            this.toDateMillis = toDateMillis;
            this.usageValue = usageValue;
            this.durationSeconds = ((toDateMillis - fromDateMillis) / 1000) + 1;
        }

        public final Date getFromDate() {
            return new Date(fromDateMillis);
        }

!SLIDE smaller
# Case class awesome
## Java (still not so awesome)
    @@@ Java
        public final Date getToDate() {
            return new Date(toDateMillis);
        }

        public final Date getStartDate() {
            return getFromDate();
        }

        public final Date getEndDate() {
            return getToDate();
        }

        public DateRange getDateRange() {
            return new DateRange(getFromDate(), getToDate());
        }

        final long getFromDateAsTime() {
            return this.fromDateMillis;
        }

        final long getToDateAsTime() {
            return this.toDateMillis;
        }

        public BigDecimal getUsageValue() {
            return this.usageValue;
        }

!SLIDE smaller
# Case class awesome
## Java (all this for a simple data structure?)

    @@@ Java
        public final long getDurationSeconds() {
            return this.durationSeconds;
        }

        public String toString() {
            if (this.stringRepresentation == null) {
                this.stringRepresentation = createStringRepresentation();
            }
            return this.stringRepresentation;
        }

        protected String createStringRepresentation() {
            return String.format("%tc - %tc, %f", getFromDate(),getToDate(),this.usageValue.doubleValue());
        }
    }

!SLIDE smaller 
# Case class awesome
## Scala

    @@@ Scala
    case class ResourceUsage(fromDateMillis:long,toDateMillis:long,usageValue:BigDecimal) 
      extends TimeWindow {

      if (toDateMillis <= fromDateMillis) {
        throw new IllegalArgumentException(toDateMillis + " must be after " + fromDateMillis);
      }
      if (usageValue == null) {
        throw new IllegalArgumentException("cannot have null usage");
      }
      val durationSeconds = ((toDateMillis - fromDateMillis) / 1000) + 1;
      def fromDate = new Date(fromDateMillis)
      def toDate = new Date(toDateMillis);
      def getStartDate = fromDate
      def getEndDate = toDate
      def dateRange = new DateRange(fromDate,toDate)
    }

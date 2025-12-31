# Chore: Add Logging to health_check.py

## Chore Description
Replace all print statements in `adws/health_check.py` with proper Python logging using appropriate log levels. This will provide better control over log verbosity, formatting, and consistency with other Python applications. The logging should preserve the exact same log output format and ensure all output is written to standard out.

## Relevant Files
Use these files to resolve the chore:

- `adws/health_check.py` - Main health check script with 19 print statements that need to be replaced with logging
  - Line 313: Header message for health check start
  - Lines 318-321: Overall status summary and timestamp
  - Lines 326-327: Check results header
  - Line 331: Individual check status line
  - Line 339: Check detail output
  - Line 342: Check error output
  - Line 344: Check warning output
  - Lines 350-352: Warnings section
  - Lines 358-360: Errors section
  - Lines 366-377: Next steps section (multiple prints)
  - Lines 384-391: Issue comment posting status messages

- `adws/trigger_cron.py` - Reference for logging pattern used in other ADW scripts
  - Shows usage of print with INFO: prefix pattern (lines 44, 59, 69, 85, 98)

- `adws/agent.py` - Another reference for error output patterns
  - Shows usage of print with file=sys.stderr for errors (line 56, 79)

## Step by Step Tasks
IMPORTANT: Execute every step in order, top to bottom.

### Step 1: Set up logging configuration in health_check.py
- Import the logging module at the top of health_check.py (add to imports section around line 32)
- Configure the root logger with a simple format that preserves the existing message structure
- Set up a stream handler to ensure output goes to stdout
- Set the default log level to INFO to show both info and error messages
- Use a format like: `'%(message)s'` to preserve the exact emoji and formatting from print statements

### Step 2: Create logger instance
- Create a module-level logger instance using `logging.getLogger(__name__)`
- Place this after the imports and before the class definitions (around line 42)

### Step 3: Replace print statements in helper functions
- Replace print statement in `_print_health_check_header()` (line 313) with `logger.info()`
- Replace print statements in `_print_overall_status_summary()` (lines 318-321) with `logger.info()`
- Replace print statements in `_print_detailed_check_results()` (lines 326-344) with appropriate logging:
  - Use `logger.info()` for header and status lines
  - Use `logger.error()` for error messages (line 342)
  - Use `logger.warning()` for warning messages (line 344)
  - Use `logger.info()` for detail output (line 339)
- Replace print statements in `_print_warnings_section()` (lines 350-352) with `logger.warning()`
- Replace print statements in `_print_errors_section()` (lines 358-360) with `logger.error()`
- Replace print statements in `_print_next_steps_section()` (lines 366-377) with `logger.info()`
- Replace print statements in `_post_health_check_results_to_issue()` (lines 384-391):
  - Use `logger.info()` for status messages (lines 384, 389)
  - Use `logger.error()` for failure message (line 391)

### Step 4: Verify no print statements remain
- Search the file for any remaining print() calls
- Ensure all have been replaced with appropriate logging calls
- Verify that f-strings and emoji formatting are preserved

### Step 5: Run validation commands
- Execute the health check script to verify logging output appears correctly on stdout
- Verify the output format matches the previous print-based output
- Run any tests that depend on health_check.py to ensure no regressions

## Validation Commands
Execute every command to validate the chore is complete with zero regressions.

- `uv run adws/health_check.py` - Run the health check script to verify all log messages appear correctly on stdout with proper formatting
- `cd app/server && uv run pytest` - Run server tests to validate no regressions in the codebase

## Notes
- The logging configuration should use a minimal format (`'%(message)s'`) to preserve the exact emoji-rich output style
- All log output must go to stdout (not stderr) for consistency with existing behavior
- The emoji characters (🏥, ✅, ❌, 📅, 📋, ⚠️, 📝, 📤) should be preserved exactly as-is in the logging output
- Consider using `logging.basicConfig()` with `stream=sys.stdout` to ensure stdout output
- The logging level should be set to INFO by default to show info, warning, and error messages
- This follows the pattern established by other ADW scripts which use direct print statements, but implements proper logging infrastructure for better maintainability

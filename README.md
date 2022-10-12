# User Bulk Action Reset Password #

Action can reset passsword for selected users, 
thats means moodle will reset password, 
moodle will generate new and notify user with new password, 
after login is user forced to change his password.

For install, copy file to moodledir/admin/user/

And put this code inside moodledir/admin/user/user_bulk_forms.php into function    public function get_actions(): array {}


if (has_capability('moodle/user:update', $syscontext)) {
            $actions['resetpassword'] = new action_link(
                new moodle_url('/admin/user/user_bulk_resetpassword.php'),
                get_string('resetpassword','bulkusers'));
        }
        
Example:

    public function get_actions(): array {

        global $CFG;

        $syscontext = context_system::instance();
        $actions = [];
        if (has_capability('moodle/user:update', $syscontext)) {
            $actions['confirm'] = new action_link(
                new moodle_url('/admin/user/user_bulk_confirm.php'),
                get_string('confirm'));
        }
        if (has_capability('moodle/site:readallmessages', $syscontext) && !empty($CFG->messaging)) {
            $actions['message'] = new action_link(
                new moodle_url('/admin/user/user_bulk_message.php'),
                get_string('messageselectadd'));
        }
        if (has_capability('moodle/user:delete', $syscontext)) {
            $actions['delete'] = new action_link(
                new moodle_url('/admin/user/user_bulk_delete.php'),
                get_string('delete'));
        }
        $actions['displayonpage'] = new action_link(
                new moodle_url('/admin/user/user_bulk_display.php'),
                get_string('displayonpage'));

        if (has_capability('moodle/user:update', $syscontext)) {
            $actions['download'] = new action_link(
                new moodle_url('/admin/user/user_bulk_download.php'),
                get_string('download', 'admin'));
        }

        if (has_capability('moodle/user:update', $syscontext)) {
            $actions['forcepasswordchange'] = new action_link(
                new moodle_url('/admin/user/user_bulk_forcepasswordchange.php'),
                get_string('forcepasswordchange'));
        }
        if (has_capability('moodle/cohort:assign', $syscontext)) {
            $actions['addtocohort'] = new action_link(
                new moodle_url('/admin/user/user_bulk_cohortadd.php'),
                get_string('bulkadd', 'core_cohort'));
        }

        // Any plugin can append actions to this list by implementing a callback
        // <component>_bulk_user_actions() which returns an array of action_link.
        // Each new action's key should have a frankenstyle prefix to avoid clashes.
        // See MDL-38511 for more details.
        $moreactions = get_plugins_with_function('bulk_user_actions', 'lib.php');
        foreach ($moreactions as $plugintype => $plugins) {
            foreach ($plugins as $pluginfunction) {
                $actions += $pluginfunction();
            }
        }

        return $actions;

    }




## License ##

2022 Lukas Celinak <lukascelinak@gmail.com>

This program is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free Software
Foundation, either version 3 of the License, or (at your option) any later
version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with
this program.  If not, see <http://www.gnu.org/licenses/>.

If you find this project useful, please consider making a donation.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://paypal.me/lukascelinak?country.x=SK&locale.x=sk_SK)

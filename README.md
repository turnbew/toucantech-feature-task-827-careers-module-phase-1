FOR PRIVACY AND CODE PROTECTING REASONS THIS IS A SIMPLIFIED VERSION OF CHANGES AND NEW FEATURES

TASK DATE : DATE: 15.02.2017 - 15.11.17

TASK'S LEVEL: (MEDIUM)

TASK SHORT DESCRIPTION: 827 [
								 827 - part - just making some changes on other modules
							]
							
GITHUB REPOSITORY CODE: feature/task-827-careers-module-phase-1

CHANGES

	IN FILES: 
	
		resources_form.php
		events_form.php
		
			ADDED CODE IN IT: 
			
				<li>
					<label for="slug"><?=sprintf(lang('news_show_joker'), strtolower(Settings::get('careers_tab')))?></label>
					<div class="input"><?=form_checkbox('show_careers_tab', 1, @$post->show_careers_tab); ?></div>
				</li>
					
			
			
		clubs_form.php

			ADDED CODE IN IT: 
			
				<li>
					<label for="slug"><?=sprintf(lang('news_show_joker'), strtolower(Settings::get('careers_tab')))?></label>
					<?=form_checkbox('show_careers_tab', 1, @$club->show_careers_tab); ?>
				</li>
								
								
		
		content_resources.php
		
			ADDED CODE IN IT: 
			
				Inside function create_resource()

					.....
					'show_careers_tab' => $this->input->post('show_careers_tab'),
					.....
					'show_careers_tab' => (isset($_POST['show_careers_tab']) ? $_POST['show_careers_tab'] : 0),
					.....
					
					
					
		
		news_form.php
		
			ADDED CODE IN IT: 
			
				<li>
					<label for="slug"><?=sprintf(lang('news_show_joker'), strtolower(Settings::get('careers_tab')))?></label>
					<div class="input"><?=form_checkbox('show_careers_tab', 1, @$post->show_careers_tab); ?></div>
				</li>
				<li>
					<label for="slug"><?php echo lang('news_show_careers_guides'); ?></label>
					<div class="input"><?=form_checkbox('show_careers_guides', 1, @$post->show_careers_guides); ?></div>
				</li>
				
		
		content.php
		
			ADDED CODE IN IT: 
			
				Inside functions create_stroy and edit_story
	
					......
					'show_careers_tab' => ($this->input->post('show_careers_tab') ?: 0),
					'show_careers_guides' => ($this->input->post('show_careers_guides') ?: 0),
					...... 
					->set('industries_list_selectbox', array()) //caused problem, I had to put in ...
					......
			

				Inside functions edit_club
			
					....
					$club['show_careers_tab'] = $this->input->post('show_careers_tab') ? 1 : 0;
					....
					
					
			
		
		news_lang.php
		
			ADDED CODE IN IT: 
			
				$lang['news_show_careers_guides']            = 'Show careers guides?';
				$lang['news_show_joker']				     = 'Show %s?';
				
				
				
		details.php
		
		
			ADDED CODE IN IT 
			
				<?php 

					defined('BASEPATH') or exit('No direct script access allowed');
				  
					/**
					 * @owner	ToucanTech
					 * @author 	Lajos Deli alias Lali <lajos@toucantech.com>
					 * @package Careers
					**/
					
					class Module_Careers extends Module 
					{

						public $version = '1.0.0';

						
						public function info() 
						{
							return array(
								'name' => array(
									'en' => 'Careers'
								),
								'description' => array(
									'en' => 'Controlling careers and mentoring pages on front and back'
								),
								'frontend' => TRUE,
								'backend' => TRUE,
							);
						}

						
						
						public function install() 
						{
							$basePath = (basename(FCPATH) == 'installer') ? dirname(FCPATH) . '/' : FCPATH;
						
							//create folders and set permissions
							@mkdir($basePath . $this->upload_path . 'careers/background', 0777, true);
							@mkdir($basePath . $this->upload_path . 'careers/background/thumbs', 0777, true);
							@mkdir($basePath . $this->upload_path . 'careers/content', 0777, true);
							@mkdir($basePath . $this->upload_path . 'careers/content/thumbs', 0777, true);

							...............
							
							return true;
						}

						
						
						public function uninstall() 
						{
							//drop careers_setup table
							$this->dbforge->drop_table('careers_setup');
							
							//delete career module fields from settings table
							$this->db->delete($this->db->dbprefix('settings'), array('module' => 'careers'));
							
							........................
							
							return TRUE;
						}

						
						
						public function upgrade($old_version) 
						{
							if(version_compare($old_version, '1.0.1', 'lt')){
								
							}
								 
							return TRUE;
						}

						
						
						public function help() 
						{
							return "No documentation has been added for this module.";
						}

						
						
						private function _create_careers_setup_table() 
						{
							return $this->db->query(
										"CREATE TABLE IF NOT EXISTS `" . $this->db->dbprefix("careers_setup") . "` ( " .
										" `id` INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY, " .
										..............
										" `updated_by` int(11) " .
										") ENGINE=InnoDB  DEFAULT CHARSET=utf8;"
									);	
						}//END function function _create_careers_setup_table



						private function _insert_into_careers_setup_table($records = array()) 
						{
							if (count($records) > 0) { 
								$table = $this->db->dbprefix('careers_setup');
								$time = time();
								foreach ($records as $key => $record) {			
									................
								}
							}
						}//END function function _insert_into_careers_setup_table		
						
						
						
						
						private function _get_records_to_careers_setup_table() 
						{
							return	array (	
								array (		
									'section' => 'welcome',
									'settings' => array(
										'switch' => true,
										'section_title' => 'Use your Alumni network for careers support',	
										'background_image' => '',
										'background_image_positioning' => 'fit',
										'background_color' => '#ffffff',
										'background_color_transparency' => '100',
										'sub_heading' => 'Welcome to the Alumni Careers Page',
										'content_photo' => '',
										'introduction_text' => '',
										'help_box_text' => 'Send a request to the careers team',
										'help_box_switch' => true,
									), 	
								), 
								array (
									'section' => 'career_resources',
									'settings' => array(
										'switch' => true,
										'section_title' => 'Career Resources',	
										'background_image' => '',
										'background_image_positioning' => 'fit',
										'background_color' => '#ffffff',
										'background_color_transparency' => '100',
									), 	
								), 
								array (
									'section' => 'career_guides',
									'settings' => array(
										'switch' => true,
										'section_title' => 'Career Guides',	
										'background_image' => '',
										'background_image_positioning' => 'fit',
										'background_color' => '#ffffff',
										'background_color_transparency' => '100',
									),
								), 
								array (
									'section' => 'mentoring',
									'settings' => array(
										'switch' => true,
										'section_title' => 'Mentoring',	
										'background_image' => '',
										'background_image_positioning' => 'fit',
										'background_color' => '#ffffff',
										'background_color_transparency' => '100',
										'sub_heading' => 'Welcome to the Mentoring Page',
										'introduction_text' => '',
									), 	
								), 
								array (
									'section' => 'featured_mentors',
									'settings' => array(
										'switch' => true,
										'section_title' => 'Featured Mentors',	
										'background_image' => '',
										'background_image_positioning' => 'fit',
										'background_color' => '#ffffff',
										'background_color_transparency' => '100',
									),
								), 
								array (
									'section' => 'jobs_and_internships',
									'settings' => array(
										'switch' => true,
										'section_title' => 'Jobs and Internships',	
										'background_image' => '',
										'background_image_positioning' => 'fit',
										'background_color' => '#ffffff',
										'background_color_transparency' => '100',
									),
								), 
								array (
									'section' => 'career_news',
									'settings' => array(
										'switch' => true,
										'section_title' => 'Career News',	
										'background_image' => '',
										'background_image_positioning' => 'fit',
										'background_color' => '#ffffff',
										'background_color_transparency' => '100',
									),
								), 
								array (
									'section' => 'career_groups',
									'settings' => array(
										'switch' => true,
										'section_title' => 'Career Groups',	
										'background_image' => '',
										'background_image_positioning' => 'fit',
										'background_color' => '#ffffff',
										'background_color_transparency' => '100',
									),
								), 
								array (
									'section' => 'career_events',
									'settings' => array(
										'switch' => true,
										'section_title' => 'Career Events',	
										'background_image' => '',
										'background_image_positioning' => 'fit',
										'background_color' => '#ffffff',
										'background_color_transparency' => '100',
									),
								), 
							);
						}//END function _get_records_to_careers_setup_table	



						private function _insert_into_settings_table($records = array()) 
						{
							if (count($records) > 0) { 
								$table = $this->db->dbprefix('settings');
								foreach ($records as $key => $record) {			
									if ( ! $this->db->value_exists($record['slug'], 'slug', $table)) {
										$this->db->insert($table, $record);
									}
								}
							}
						}//END function function _insert_into_settings_table		
					

								

						private function _get_records_to_settings_table() 
						{
							return	array (	
								array(
									'slug' => 'careers_tab',
									'title' => 'Careers menu caption',
									'description' => 'Caption of the Careers menu on front',
									'`default`' => 'Careers',
									'`value`' => 'Careers',
									'type' => 'text',
									'`options`' => '',
									'is_required' => 0,
									'is_gui' => 1,
									'module' => 'careers',
								),
								array(
									'slug' => 'careers_toggle',
									'title' => 'Careers toggle',
									'description' => 'Toggle the Careers for the front end',
									'`default`' => '0',
									'`value`' => '0',
									'type' => 'radio',
									'`options`' => '1=Enabled|0=Disabled',
									'is_required' => 0,
									'is_gui' => 1,
									'module' => 'careers',
								),
							);
						}//END function _get_records_to_settings_table	


						
						
						public function add_boolean_field_to_table($table, $field, $null = false, $default = null, $after = '') 
						{
							if (!$this->db->field_exists($field, $table)) {
								..............
								return $this->db->query($sql);
							}
						}//END function add_boolean_field_to_table
					
					
					
					
						public function delete_directory_and_files($directory)
						{
							if ($handle = opendir($directory)) {
								while (( $file = readdir($handle)) !== false ) {
									................
								}
							}
							
							closedir($handle);
						}//END function delete_directory_and_files
						
						
						
					} //END class Module_Careers

				?>
